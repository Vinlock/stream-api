PHP API Wrapper for multiple streaming services.

**Note:** Update coming, as Twitch has updated their API making this currently obsolete.

#### Supported Service Providers
* [Twitch.tv](http://www.twitch.tv)
* [Hitbox.tv](http://www.hitbox.tv)

## Install via Composer

```shell
$ composer require vinlock/stream-api
```

## Usage

### Merging games.
```php
$twitch = \Vinlock\StreamAPI\Services\Twitch::game("Dota 2");

$hitbox = \Vinlock\StreamAPI\Services\Hitbox::game("Dota 2");

$merge = \Vinlock\StreamAPI\Services\Service::merge($twitch, $hitbox);

echo $merge->getJSON(); // Displays all streams from highest to lowest viewers for Dota 2 on Twitch and Hitbox.
```
Merge as many games as you want...
```php
$bladeandsoul_twitch = \Vinlock\StreamAPI\Services\Twitch::game("Blade and Soul");
$bladeandsoul_hitbox = \Vinlock\StreamAPI\Services\Hitbox::game("Blade and Soul");
$overwatch_twitch = \Vinlock\StreamAPI\Services\Twitch::game("Overwatch");
$overwatch_hitbox = \Vinlock\StreamAPI\Services\Hitbox::game("Overwatch");

$merge = \Vinlock\StreamAPI\Services\Service::merge(
    $bladeandsoul_twitch,
    $bladeandsoul_hitbox, 
    $overwatch_twitch, 
    $overwatch_hitbox
);
```
Or you may pass in the games as an array.
```php
$games = [
    "Blade and Soul",
    "Overwatch",
    "Aion Online"
];

$twitch = \Vinlock\StreamAPI\Services\Twitch::game($games);
$hitbox = \Vinlock\StreamAPI\Services\Hitbox::game($games);

$merge = \Vinlock\StreamAPI\Services\Service::merge($twitch, $hitbox);
// or pass the Service Objects as an array to be merged.
$merge = \Vinlock\StreamAPI\Services\Service::merge( [ $twitch, $hitbox ] );

echo $merge->getJSON();
```

### From Usernames
Results will only show for online users.
```php
$twitch = new \Vinlock\StreamAPI\Services\Twitch("vinlockz");
$hitbox = new \Vinlock\StreamAPI\Services\Hitbox("vinlock");
```
Or pass many usernames as an array.
```php
$twitch_streams = [ "trick2g", "vinlockz", ... ];
$hitbox_streams = [ "hitboxstream1", "hitboxstream2", ... ];

$twitch = new \Vinlock\StreamAPI\Services\Twitch($twitch_streams);
$hitbox = new \Vinlock\StreamAPI\Services\Twitch($hitbox_streams);
```
Then merge these as well.
```php
$merge = \Vinlock\StreamAPI\Services\Service::merge($twitch, $hitbox);
// or
$merge = \Vinlock\StreamAPI\Services\Service::merge( [ $twitch, $hitbox ] );

echo $merge->getJSON();     // Displays the information for all streams merged as JSON.
```

### Universal Merging
You may merge instances initialized by usernames with ones initialized by Games.
```php
$bladeandsoul_twitch = \Vinlock\StreamAPI\Services\Twitch::game("Blade and Soul");
$twitch = new \Vinlock\StreamAPI\Services\Twitch("vinlockz");

$merge = \Vinlock\StreamAPI\Services\Service::merge($twitch, $bladeandsoul_twitch);

$merge->getJSON();
```

### Data Retrieval
You can get an array, JSON, or object from the Service Object.
```php
echo $merge->getJSON();     // Displays the information for all streams merged as JSON.
echo $merge->getArray();    // Array
echo $merge->getObject();   // Object
```

## Example JSON
This will be universal for every stream provider.
```json
[
    {
        "username": "trick2g",
        "display_name": "Trick2g",
        "game": "League of Legends",
        "preview": {
            "small": "https://static-cdn.jtvnw.net/previews-ttv/live_user_trick2g-80x45.jpg",
            "medium": "https://static-cdn.jtvnw.net/previews-ttv/live_user_trick2g-320x180.jpg",
            "large": "https://static-cdn.jtvnw.net/previews-ttv/live_user_trick2g-640x360.jpg"
        },
        "status": "100% Advan | Silver Clown Fiesta 101 How to TDM #Na Throws @Trick2g Day 35 No Sodabull",
        "bio": "Opening the Gates",
        "url": "https://www.twitch.tv/trick2g",
        "viewers": 4040,
        "id": 28036688,
        "avatar": "https://static-cdn.jtvnw.net/jtv_user_pictures/trick2g-profile_image-291046f75304f006-300x300.jpeg",
        "service": "twitch",
        "followers": 996140,
        "created_at": "05-31-2016 01:15:12",
        "updated_at": "05-31-2016 03:36:19"
    }
]
```
