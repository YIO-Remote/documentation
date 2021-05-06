---
title: "Creating integrations"
description: "Integrations are the interface between YIO-remote and the outside world."
menu:
  developers:
    parent: "home"
weight: 50
---

Integrations are the interface between YIO-remote and the outside world.
Using the "smarthome - typical" openHAB integration as example the necessary pieces of an integration are explained.

## Plugin Project Setup

### Environment Setup

To develop a YIO Remote integration plugin the [integrations.library](https://github.com/YIO-Remote/integrations.library/tree/develop) project needs to be checked out and made accessible for the plugin project.  
We recommend to check out `integrations.library` on the same level as the plugin project. I.e. the library can be referenced with the default path `../integrations.library`.  
This is the only mandatory dependency of an integration plugin project.

Example for an imaginary FooBar plugin:

    ~/projects
    └── yio
        ├── integration.foobar
        ├── integrations.library
        └── remote-software

For other non-standard project setups there's the `YIO_SRC` environment variable. It needs to be initialized to the base directory of the integrations.library project.

### Plugin Project File

Blueprint to include one or more integration Qt project includes into the plugin project:

    INTG_LIB_PATH = $$(YIO_SRC)
    isEmpty(INTG_LIB_PATH) {
        INTG_LIB_PATH = $$clean_path($$PWD/../integrations.library)
        message("Environment variables YIO_SRC not defined! Using '$$INTG_LIB_PATH' for integrations.library project.")
    } else {
        INTG_LIB_PATH = $$(YIO_SRC)/integrations.library
        message("YIO_SRC is set: using '$$INTG_LIB_PATH' for integrations.library project.")
    }

    ! include($$INTG_LIB_PATH/yio-plugin-lib.pri) {
        error( "Cannot find the yio-plugin-lib.pri file!" )
    }

## The plugin class - instance factory

The plugin class is used to setup an integration. Its **create** function is used to create one or more integration instances. An integration should used categorized logging, which can be controlled from outside to adjust the log levels using **setLogEnabled** function. A reference of the _\_log_ object must be propagated to the integration instances.
The category name must be equal to the name of the plugin.

**Create** function parameters :

- **config** the configuration parameters of the integration
- **entities** all entities (cast to **EntitiesInterface**)
- **notifications** (cast to **NotificationsInterface**)
- **api** (cast to **YioAPIInterface**)
- **configObj** whole config.json (cast to **ConfigInterface**)

Sample :

    class OpenHABPlugin : public PluginInterface
    {
        Q_OBJECT
        Q_PLUGIN_METADATA(IID "YIO.PluginInterface" FILE "openhab.json")
        Q_INTERFACES(PluginInterface)
    public:
        explicit OpenHABPlugin (QObject* parent = nullptr) :
            _log("openhab")
        {
            Q_UNUSED(parent)
        }
        virtual ~OpenHABPlugin () override
        {}
        void        create         (const QVariantMap& config, QObject *entities, QObject *notifications, QObject* api,
                                    QObject *configObj) override;
        void        setLogEnabled  (QtMsgType msgType, bool enable) override
        {
            _log.setEnabled(msgType, enable);
        }
    private:
        QLoggingCategory    _log;
    };

The **create** functions creates the integration instances, calls their **setup** function and emits **createDone** with information about all instances.

Sample :

    void OpenHABPlugin::create(const QVariantMap &config, QObject *entities, QObject *notifications, QObject *api, QObject *configObj)
    {
        QMap<QObject *, QVariant>   returnData;
        QVariantList                data;

        for (QVariantMap::const_iterator iter = config.begin(); iter != config.end(); ++iter) {
            if (iter.key() == "data") {
                data = iter.value().toList();
                break;
            }
        }
        for (int i = 0; i < data.length(); i++)
        {
            OpenHAB* openhab = new OpenHAB(_log, this);
            openhab->setup(data[i].toMap(), entities, notifications, api, configObj);
            QVariantMap d = data[i].toMap();
            d.insert("type", config.value("type").toString());
            returnData.insert(openhab, d);
        }
        if (data.length() > 0)
            emit createDone(returnData);
    }

## The integration instance class

The integration instance class must derive from the **Integration class**. It provides some features common for all integrations:

- Properties
  - **state** (CONNECTED, CONNECTING, DISCONNECTED) must be set by the integration calling **setState**. This function implicetly sets the **connected** state of all entities of the integration.
  - **integrationId** and **friendlyName**, both are set by the Integration::setup function which **must** be called by the integrations setup function.
  - **m_entities** is set to the **EntitiesInterface**, which can be used to access all entities.
- Virtual Functions
  - **connect** (must be overriden)
  - **disconnect** (must be overriden)
  - **enterStandby** (overriden if required)
  - **leaveStandby** (overriden if required)
  - **sendCommand** gets the commands (must be overriden)
- **setup** function, must be called by integrations setup function
- **setState** function indicates change of connection state, must be called by the integration instance. It implicitelly set the "connected" state of every entity belonging to the integration.

Sample :

    void OpenHAB::setup (const QVariantMap& config, QObject *entities, QObject *notifications, QObject* api, QObject *configObj)
    {
        Integration::setup (config, entities);

The function **connect** is called at startup and when Wifi connection is reestablished. **disconnect** is called before WiFi is turned off. **enterStandby** and **leaveStandby** are called when the standby mode changes. Especially for polling integrations we recommend to adjust the polling interval when the YIO remote is standby to save power.

A really smart integration should check, if it gets information for every entity belonging to it. If not, this entity should be set to **not connected**.

## Performance Considerations

While the UI uses CPU only when there is some user interaction, integrations are running permanently. Paying attention to performance is not primarily necessary to gain speed but to save battery power. Here some recommendations :

- Don't work with **strings**, use **enums**. They are at minimum hundred times faster.
- Don't rely on compiler optimization. Build **intermediate results** instead of computing again and again the same chains of variantMap.value("xxx").toMap().value("yyy").toMap().value("zzz")
- Return **references** (&) whereever possible instead of copying complex objects like _QVariantMap_, _QStringList_, etc.
- Use **enter/leaveStandby**
- Reuse "expensive* objects like *QNetworkAccessManager\*
- Use **dynamically allocated objects** only if necessary, it helps also to avoid memory leaks.
- Implement **feature rich classes** instead of implementing multiple used functionalities outside the class
- Processing **JSON** data with **QJSonObject** and **QJsonArray** is faster then taking the detour with **QVariantMap** and **QVariantList**
