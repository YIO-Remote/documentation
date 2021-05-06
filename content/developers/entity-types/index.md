---
title: "Entity types"
description: "Entities are the central interface objects between the integrations and the YIO-Remote UI."
menu:
  developers:
    parent: "home"
weight: 60
---

Entities are the central interface objects between the _integrations_ and the YIO-Remote UI.

They can be accessed by the _integrations_ DLLs/shared libraries using interfaces. On the other side they are designed to be used comfortably by the YIO-Remote QML based UI.
**Integrations** are something like drivers connecting smart home hubs, media servers...

The sources are in the _sources/entities_ subtree.

- The **Entities** container is used as anchor to get all the entity objects
- The **Entity** object is the base class for all concrete entity objects. An entity is identified by **entity_id**.
- The "concrete" entity objects derived from _entity_, like **Light**, **Blind**, ...

**Attributes** of an entity are all properties of an entity which can be set (updated) by the integrations.

**Features** of an entity are a set of functionalities a specific entity supports. It is used by the UI to adapt the visual representation of the entity and often by the integration to decide which attributes it has to deliver.

**Commands** of an entity contains all possible commands which can be sent from the UI to the integrations.

## Entities container

- in QML it is installed as context property **"entities"**
- in C++ you get it using _Entities::getInstance()_
- Integrations get the entities interface **EntitiesInterface** in their create function.

#### Functions provided by EntitiesInterface

    // get entites by type
    QList<EntityInterface *>    getByType           (const QString& type);

    // get entites by area
    QList<EntityInterface *>    getByArea           (const QString& area);

    // get entities by integration
    QList<EntityInterface *>    getByIntegration    (const QString& integration);

    // update an entity's attributes (not recommended, use entity function updateAttrByIndex)
    void                        update              (const QString& entity_id, const QVariantMap& attributes);

    // set connected, indicates that an entity is "online"
    void                        setConnected        (const QString& integationId, bool connected);

    // get entity interface by entity_id
    EntityInterface*            getEntityInterface  (const QString& entity_id);

#### Properties available in QML

    QList<QObject*> list                           // all entity objects
    QStringList     supported_entities             // supported entity types (light, blind)
    QStringList     supported_entities_translation // supported translations
    QStringList     loaded_entities                // all entity_ids
    QList<QObject*> mediaplayersPlaying            // playing mediaplayer entities

#### Additional functions available in QML

    void            load                           // load entities from config.json
    QObject*        get                            // get entity object by entity_id
    void            add                            // add a loaded entity, assign its integration
    void            addLoadedEntity                // add a loaded entities id
    QString         getSupportedEntityTranslation  // get the supported translation

## Generic entity object

The generic **entity** object implements a featurs common for all types of entities.
It is the base class of "concrete" entity objects like _light_, _blind_, \*mediaplayer', ..

- For the _integrations_ **EntityInterface** is used to access the entity. You get it using
  _entitiesInterface->getEntityInterface(entityId)_
- in QML it is accessed using _entities.get(enityId)_
- The generic **entity** objects contains

#### Functions provided by EntityInterface

Access to the **generic attributes** from config.json

    QString         type();                 // light, blind, ...
    QString         area();                 // room name
    QString         friendly_name();        // name used in UI
    QString         entity_id();            // unique id
    QString         integration();          // integration name
    QObject*        integrationObj();       // integration object
    QStringList     supported_features();   // list of supported features, use isSupported if possible

    bool            isSupported(int feature); // Use enum F_xxx, defined in the "concrete" entity interface

Handling of **favorite** feature

    // favorite state (can be changed by the UI)
    bool            favorite();
    void            setFavorite(bool value);

Handling of **connected** state. connected indicates that the entity is "online".

    bool            connected();
    void            setConnected(bool value);

**State** is an attribute common for all entity types. It is represented by an **enum** (ON, OFF, OPEN ...) defined by the "concrete" entity type.
Instead of the integer **enum** also the text can be used.

    int             state();
    bool            setState(int value);
    QString         stateText();            // state as text
    bool            setStateText(const QString& stateText);
    QStringList     allStates();            // All states
    bool            isOn();                 // common state interpretation
    bool            supportsOn();           // entity has no on state
    void            turnOn();               // common **ON** command
    void            turnOff();              // common **OFF** command

Update **entity** attributes (STATE, BRIGHTNESS, POSITION ...). The attribute index as **enum** is defined by "concrete" entity type.
But there is also a conversion betwen attribute name as text and index.
Update of attributes is by attribute index or name. A set of attributes can be applied using a map of named attributes.
The fasted method is of course update by index. The "concrete" entity interfaces defines enums for the state, the **attribute index**, the **command**, and the **feature**. Conversion functions between enum and strings are available.

    bool            update              (const QVariantMap& attributes);
    bool            updateAttrByName    (const QString& attrName, const QVariant& value);
    bool            updateAttrByIndex   (int attrIndex, const QVariant& value);
    QString         getAttrName         (int attrIndex);
    int             getAttrIndex        (const QString& attrName);
    QStringList     allAttributes       ();

    QString         getFeatureName      (int featureIndex);
    int             getFeatureIndex     (const QString& featureName);
    QStringList     allFeatures         ();

    QString         getCommandName      (int commandIndex);
    int             getCommandIndex     (const QString& commandName);
    QStringList     allCommands         ();

Every "concrete" entity type has a specific interface (LightInterface ...) which can be used to access the entity specific attributes.

    void*           getSpecificInterface()     // LightInterface, ...

#### Additional functions available in QML

    // Generic command to integration (use enums C_Xxx for command, as defined in the concrete interface)
    void           command (int command, const QVariant& param);

To use the defined enums in QML import the definitons, sample:

    import Entity.Light 1.0

## Concrete entity objects

A concrete entity object like **Light** (lets use the "light" as example) derives from **Entity**.
Its interface **LightInterface** contains access to the specific attributes for the _integrations_.
The same include file also contains the enums for the attributes (=attributeIndex) and states.
It was neccessary to hold the enum definitions in a separate class to use Qt's metadata based conversion of enums
between enum integer and text. And to make the enums evailable in QML.

### Light entity

    States enum :         OFF = 0, ON = 1
    Attributes enum :     STATE, BRIGHTNESS, COLOR, COLORTEMP
    Features enum :       F_BRIGHTNESS, F_COLOR, F_COLORTEMP
    Commands enum:        C_OFF, C_ON, C_TOGGLE, C_BRIGHTNESS, C_COLOR, C_COLORTEMP
    specific attributes : brightness, color, colorTemp

Specific commands available in QML

    void setBrightness    (int value);
    void setColor         (QColor value);
    void setColorTemp     (int value);

### Blind entity

    States enum :         OPEN = 0, CLOSED = 1
    Attributes enum :     STATE, POSITION
    Features enum :       F_OPEN, F_CLOSE, F_STOP, F_POSITION
    Commands enum :       C_OPEN, C_CLOSE, C_STOP, C_POSITION
    specific attributes : position

Specific commands available in QML

    void close();
    void open();
    void stop();
    void setPosition(int value);

### Remote entity

This entity is used to replace IR remote controls. They have no attributes, only commands.

    States enum :         OFFLINE = 0, ONLINE = 1
    Attributes enum :     STATE
    Features enum :
        // Transport and mediacontrols
        F_PLAY, F_PAUSE, F_PLAYTOGGLE, F_STOP, F_FORWARD, F_BACKWARD, F_NEXT, F_PREVIOUS, F_INFO, F_RECORDINGS, F_RECORD, F_LIVE,
        // digits
        F_DIGIT_0, F_DIGIT_1,F_DIGIT_2, F_DIGIT_3, F_DIGIT_4, F_DIGIT_5, F_DIGIT_6, F_DIGIT_7, F_DIGIT_8, F_DIGIT_9, F_DIGIT_10, F_DIGIT_10plus,
        F_DIGIT_11, F_DIGIT_12, F_DIGIT_SEPARATOR, F_DIGIT_ENTER,
        // navigation
        F_CURSOR_UP, F_CURSOR_DOWN, F_CURSOR_LEFT, F_CURSOR_RIGHT, F_CURSOR_OK, F_BACK, F_HOME, F_MENU, F_EXIT, F_APP, // INFO is repeated twice
        // power
        F_POWER_OFF, F_POWER_ON, F_POWER_TOGGLE,
        //tuner
        F_CHANNEL_UP, F_CHANNEL_DOWN, F_CHANNEL_SEARCH, F_FAVORITE, F_GUIDE,
        // interactive
        F_FUNCTION_RED, F_FUNCTION_GREEN, F_FUNCTION_YELLOW, F_FUNCTION_BLUE, F_FUNCTION_ORANGE,
        // video
        F_FORMAT_16_9, F_FORMAT_4_3, F_FORMAT_AUTO,
        // volume
        F_VOLUME_UP, F_VOLUME_DOWN, F_MUTE_TOGGLE,
        // input
        F_INPUT_TUNER_1, F_INPUT_TUNER_2, F_INPUT_TUNER_X, F_INPUT_HDMI_1, F_INPUT_HDMI_2, F_INPUT_HDMI_X, F_INPUT_X_1, F_INPUT_X_2,
        // output
        F_OUTPUT_HDMI_1, F_OUTPUT_HDMI_2, F_OUTPUT_DVI_1, F_OUTPUT_AUDIO_X, F_OUTPUT_X,
        // services
        F_SERVICE_NETFLIX, F_SERVICE_HULU

    Commands enum:
        // Not entity related but mus be defined somewhere
        C_REMOTE_CHARGED, C_REMOTE_LOWBATTERY,
        // Transport and mediacontrols
        C_PLAY, C_PAUSE, C_PLAYTOGGLE, C_STOP, C_FORWARD, C_BACKWARD, C_NEXT, C_PREVIOUS, C_INFO, C_RECORDINGS, C_RECORD, C_LIVE,
        // digits
        C_DIGIT_0, C_DIGIT_1,C_DIGIT_2, C_DIGIT_3, C_DIGIT_4, C_DIGIT_5, C_DIGIT_6, C_DIGIT_7, C_DIGIT_8, C_DIGIT_9, C_DIGIT_10, C_DIGIT_10plus,
        C_DIGIT_11, C_DIGIT_12, C_DIGIT_SEPARATOR, C_DIGIT_ENTER,
        // navigation
        C_CURSOR_UP, C_CURSOR_DOWN, C_CURSOR_LEFT, C_CURSOR_RIGHT, C_CURSOR_OK, C_BACK, C_HOME, C_MENU, C_EXIT, C_APP, // INFO is repeated twice
        // power
        C_POWER_OFF, C_POWER_ON, C_POWER_TOGGLE,
        //tuner
        C_CHANNEL_UP, C_CHANNEL_DOWN, C_CHANNEL_SEARCH, C_FAVORITE, C_GUIDE,
        // interactive
        C_FUNCTION_RED, C_FUNCTION_GREEN, C_FUNCTION_YELLOW, C_FUNCTION_BLUE, C_FUNCTION_ORANGE,
        // video
        C_FORMAT_16_9, C_FORMAT_4_3, C_FORMAT_AUTO,
        // volume
        C_VOLUME_UP, C_VOLUME_DOWN, C_MUTE_TOGGLE,
        // input
        C_INPUT_TUNER_1, C_INPUT_TUNER_2, C_INPUT_TUNER_X, C_INPUT_HDMI_1, C_INPUT_HDMI_2, C_INPUT_HDMI_X, C_INPUT_X_1, C_INPUT_X_2,
        // output
        C_OUTPUT_HDMI_1, C_OUTPUT_HDMI_2, C_OUTPUT_DVI_1, C_OUTPUT_AUDIO_X, C_OUTPUT_X,
        // services
        C_SERVICE_NETFLIX, C_SERVICE_HULU

Specific commands available in QML

    // transport and media controls
    void play();
    void pause();
    void playToggle();
    void stop();
    void forward();
    void backward();
    void next();
    void previous();
    void info();
    void recordings();
    void record();
    void live();

    // navigation
    void cursorUp();
    void cursorDown();
    void cursorLeft();
    void cursorRight();
    void cursorOK();
    void back();
    void home();
    void menu();
    void exit();
    void app();

    // power commands
    void powerOn();
    void powerOff();
    void powerToggle();

    // tuner commands
    void channelUp();
    void channelDown();
    void channelSearch();
    void toFavorite();
    void guide();

    // volume commands
    void volumeUp();
    void volumeDown();
    void muteToggle();

### MediaPlayer entity

    States enum :         OFF = 0, ON = 1, IDLE = 2, PLAYING = 3
    Attributes enum :     STATE, SOURCE, VOLUME, MUTED, MEDIATYPE, MEDIATITLE, MEDIAARTIST, MEDIAIMAGE, MEDIADURATION, MEDIAPROGRESS
    Features enum :       F_APP_NAME, F_MEDIA_ALBUM, F_MEDIA_ARTIST, F_MEDIA_DURATION,  F_MEDIA_POSITION, F_MEDIA_IMAGE,
                          F_MEDIA_PROGRESS, F_MEDIA_TITLE, F_MEDIA_TYPE, F_MUTE, F_MUTE_SET, F_NEXT, F_PAUSE, F_PLAY,
                          F_PREVIOUS, F_SEARCH, F_SEEK, F_SHUFFLE, F_SOURCE, F_STOP, F_TURN_OFF,
                          F_TURN_ON, F_VOLUME, F_VOLUME_DOWN, F_VOLUME_SET, F_VOLUME_UP, F_LIST, F_SPEAKER_CONTROL

    Commands enum :       C_TURNOFF, C_TURNON, C_PLAY, C_PAUSE, C_STOP, C_PREVIOUS, C_NEXT, C_VOLUME_SET, C_VOLUME_UP,
                          C_VOLUME_DOWN, C_MUTE, C_BROWSE, C_SEARCH, C_SEARCH_ITEM, C_PLAY_ITEM, C_GETALBUM, C_GETPLAYLIST

     specific attributes : source, volume, muted, mediaDuration, mediaProgress, mediaType, mediaTitle, mediaArtist, mediaImage

Specific commands available in QML

    void play();
    void pause();
    void stop();
    void previous();
    void next();
    void setVolume(int value);
    void volumeUp();
    void volumeDown();

extensions for media browsing

    // browse item_key, "TOP", "BACK", "PLAY"
    void browse (QString command);

    // command PLAY, QUEUE, SHUFFLE, RADIO
    void playMedia (const QString& command, const QString& itemKey);

    // Search
    void search (const QString& searchText, const QString& itemKey);

Mediaplayer uses **Qt's** _QAbstractListModel_ to deliver browse results.
The model is delivered to the remote using the function

    void setBrowseModel(QObject* model);
    void setSearchModel(QObject* model);

A list item consists of

    QString item_key;         // unique key
    QString title;            // like artist, ...
    QString sub_title;        // like album, ..
    QString image_url;        // album art
    QString input_prompt;     // for searchable items

The browse result also contains a header

    QString type;             // type of browse result (Artists, Albums, Tracks ...)
    QString title;            // artist name ...
    int     level;            // hierarchy level

and a list of play commands which can be applied to the list items

    QStringList playCommands; // PLAY, SHUFFLE, RADIO

### Weather entity

    Stated enum :         OFFLINE = 0, ONLINE = 1
    Attributes enum :     STATE, CURRENT, FORECAST
    Features enum :       F_DETAIL
    Commands enum :       C_SETLOCATION
    specific attributes : current, forecast

Current is a structure of weatherdata, forecast is a list of up to 5 such structures.

A structure contains

    QString     date;         // Short name of the day
    QString     description;  // Short weather description "mostly cloudy"
    QString     imageurl;     // Icon url
    QString     temp;         // Current, min, max of the day temperature including unit
    QString     rain;         // Rain in  mm
    QString     snow;         // snow in mm
    QString     wind;         // wind in km/h
    QString     humidity;     // humidity in %

The value strings contains the values including unit (°C). To set the values integration can use this functions.

    void setCurrent (const WeatherItem &current);
    void setForecast (QObject *model);       // Model derives from  QAbstractListModel

}

## Code samples

#### Integration code snippets

    #include "../remote-software/sources/entities/entitiesinterface.h"
    #include "../remote-software/sources/entities/entityinterface.h"

    // entities from setup
    m_entities = qobject_cast<EntitiesInterface *> (entities);
    EntityInterface* entity = m_entities->getEntityInterface("SomfyShutter|Left");

    // set state and attribute using state text and attribute text
    entity->setStateText("CLOSED");
    entity->updateAttrByName("position", 77);

    // set attribute using index
    int index = entity->getAttrIndex("position");
    entity->updateAttrByIndex(index, 66);

Using "concrete" interface

    #include "../remote-software/sources/entities/blindinterface.h"

    BlindInterface* blind = static_cast<BlindInterface*>(entity->getSpecificInterface());
    // set state
    entity->setState(BlindDef::CLOSED);
    // or
    entity->updateAttrByIndex(BlindDef::STATE, BlindDef::CLOSED);

    // get current position
    pos = blind->position();
    // set position
    entity->updateAttrByIndex(BlindDef::POSITION, 88);

#### Using concrete entity enums in QML

To use the enums the registered object must be imported :

    import Entity.MediaPlayer 1.0

    if (obj.state == MediaPlayer.PLAYING) {
    ...
    }
