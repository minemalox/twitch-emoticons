# twitch-emoticons
Get a Twitch channel's Twitch emotes and BTTV emotes!
```js
const emoticons = require('twitch-emoticons');

// Example for getting Twitch emotes:
emoticons.emote('Kappa').then(emote => {
    console.log(emote.toLink(0));
    // "https://static-cdn.jtvnw.net/emoticons/v1/25/1.0"
});

// Example for getting BTTV emotes:
emoticons.emote('thefreaking2:gachiGASM').then(emote => {
    console.log(emote.toLink(1));
    // "https://cdn.betterttv.net/emote/55999813f0db38ef6c7c663e/2x"
});

// Example for getting channels:
emoticons.channel('cirno_tv').then(channel => {
    console.log(channel.emotes);
    // Cache {...}
})

// Example for parsing text:
let parsed = emoticons.parse('Hi <Kappa>, <PogChamp>!!!', 'html', 2, '<', '>');
console.log(parsed);
// "Hi <img class="twitch-emote twitch-emote-2 src=https://static-cdn.jtvnw.net/emoticons/v1/25/3.0">, <img class="twitch-emote twitch-emote-2 src=https://static-cdn.jtvnw.net/emoticons/v1/88/3.0">!!!"
```

# Documentation
### Emotes
*Methods related to getting an emote or the Emote class.*

- Due to how BTTV works, non-global BTTV emotes should be accessed like so:  
`channelName:emoteName`  
Once it has been added to the cache, this will not be necessary.  
- twitch-emoticons also provide TWITCH\_GLOBAL and BTTV\_GLOBAL for prefixing:  
`TWITCH_GLOBAL + ':emoteName'`  
This is not necessary at all, but may be useful.  

##### emote(emoteName)
`emoteName` The name of the emote.  
If the emote is not found in cache, twitch-emoticons will attempt to find it and cache the channel and its emotes (Twitch and BTTV).  
Returns a Promise containing the Emote object.

##### getEmote(emoteName)
`emoteName` The name of the emote.  
Returns an Emote object, if it is in the cache.

##### \<Emote\>
`id` The emote id.  
`name` Name of the emote.  
`channel` Channel this emote belongs to. Will be null for non-global BTTV emotes.  
`set` Set ID of the emote. For BTTV emotes, this would be the creator channel's name (does not guarantee that said channel has the emote).  
`description` Description of the emote. Only available for Twitch global emotes.  
`bttv` Whether or not this emote is a BTTV emote.  
`gif` Whether or not this emote is a GIF.

##### \<Emote\>.toLink([size])
`size` Size of the image, 0 to 2.  
Returns a link to the emote's image.

### Channels
*Methods related to getting a channel or the Channel class.*

##### channel([channelName])
`channelName` The name of the Twitch channel. Leave null for the global Twitch channel.  
If the channel is not found in cache, twitch-emoticons will attempt to find it and cache it and its emotes (Twitch and BTTV).  
Returns a Promise containing the Channel object.

##### getChannel([emoteName])
`channelName` The name of the Twitch channel. Leave null for the global Twitch channel, if cached.  
Returns a Channel object, if it is in the cache.

##### \<Channel\>
`name` The channel name.  
`emotes` A Cache of emotes this channel has.

### Parsing
*These are methods that parses text.*

- Parsing can parse strings to one of these types:
`html` which includes a class for the emote, and the emote size.  
`markdown` which has the emote name as the alt-text.  
`bbcode` for message boards.  
`plain` for just a link.  
For custom formats, you can use {link}, {name}, and {size}.  
For example, "[[[{link} {name}]]]" will output as "[[[https://static-cdn.jtvnw.net/emoticons/v1/25/1.0 Kappa]]]".
This can be set as a string in the `custom` argument.  

##### parse(text[, type, size, start, end, custom])
`text` Text to parse.  
`type` Type of parsing.  
`size` Size of the image, 0 to 2.  
`start` Character(s) to open an emote.  
`end` Character(s) to close an emote.  
`custom` If set, `type` is ignored and uses this custom format.  
Only emotes that are cached will be formatted.  
Returns the formatted string.

##### parseAll(text[, type, size, start, end, custom])
`text` Text to parse.  
`type` Type of parsing.  
`size` Size of the image, 0 to 2.  
`start` Character(s) to open an emote.  
`end` Character(s) to close an emote.  
`custom` If set, `type` is ignored and uses this custom format.  
Caution! This method goes through every word and checks the APIs for an emote.  
It is recommended that you use parse() instead.  
Returns a Promise containing the formatted string.

### Caching
*These are methods that interact with the caches, where channels and emotes are stored.*

##### loadChannel([channelName])
`channelName` The name of the Twitch channel. Leave null for global Twitch emotes.  
Caches a Twitch channel's emotes. This only includes Twitch emotes.  
Returns a Promise containing the Channel object.

##### loadChannels(channelNames)
`channelNames` Array of Twitch channel names.  
Caches multiple Twitch channels' emotes. This only includes Twitch emotes.  
Returns a Promise containing an array of Channel objects.

##### loadAllChannels()
Caches all Twitch channel emotes.  
This takes a long time, but use it if you do not want to poll the Twitch Emotes API over and over.  
Returns a Promise containing an array of Channel objects.

##### loadBTTVChannel([channelName])
`channelName` The name of the Twitch channel. Leave null for global BTTV emotes.  
Caches a Twitch channel's emotes. This only includes BTTV emotes.  
Returns a Promise containing the Channel object.

##### loadBTTVChannels(channelNames)
`channelNames` Array of Twitch channel names.  
Caches multiple Twitch channels' emotes. This only includes BTTV emotes.  
Returns a Promise containing an array of Channel objects.

##### cache()
This methods gets you a copy of the caches.  
Returns an object with `channels`, `emotes`, and `bttvEmotes` Caches.

##### clearCache()
Clears the caches completely.

##### \<Cache\>
Extends Map. Has `find()`, `filter()`, and `map()` for utility purposes.

### Changelog
##### 1.5
- Exposed Emote, Channel, and Cache classes
- loadBTTVChannels()
- loadAllChannels()
- More properties for Emotes.
- Parsing can now have a custom format.
- Some other stuff.

##### 1.0 - 1.4
- Initial release and subsequent developments.
- (Yes, I know this is a terrible changelog.)

### More Info
twitch-emoticons uses the [Twitch Emotes API](https://twitchemotes.com/) and the BetterTTV API.  
Found a bug or something does not work as expected? Open up an issue on [Github](https://github.com/1Computer1/twitch-emoticons)!