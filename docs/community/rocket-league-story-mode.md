![screenshot](/img/rocket-league-story-mode/screenshot.png)

## Playing Rocket League Story Mode

To get started with Story Mode, install [RLBotGUI](https://github.com/RLBot/RLBotGUI#installation).

Once installed, go to Add->Download Botpack to download the community's bots. 
Once they are downloaded you can click on "Story Mode" on the top right.

## Upgrades and Teammates

After you win a challenge, you get rewarded 1 currency. You can use it to purchase upgrades
or recruit an available teammate. After you win all challenges in a city, all opponents from that
city become available to recruit.

Some matches require you to have teammates. If you don't have enough teammates and
no currency, then you can play previous matches to earn some more currency.

## Choosing Difficulty

When you start Story Mode, you can choose your difficulty between easy, default and custom.

- Easy is best for **gold/plat** players
- Default is best for **diamond/champ** players

Custom allows you to provide a JSON file that is used as the story configuration. [This](https://github.com/RLBot/RLBotGUI/blob/master/rlbot_gui/story/story-default.json) is what the default configurations looks like.

Here are some example custom configurations:

- [Psyonix-only bots](https://gist.github.com/azeemba/b45b553ea3d87d057e99a18d37084c13): Best for **bronze/silver** players who find the default bots too difficult
- [No teammates/Unfair](https://gist.github.com/azeemba/e858750fc36616ab8ca313d5f17b9f7b): Best for players who find "default" configuration too easy or find bot teammates annoying.

## Creating Custom Configurations

[This](https://github.com/RLBot/RLBotGUI/blob/master/rlbot_gui/story/story-default.json) is the actual story configuration used in Story Mode so you can use that as a reference to create your own custom configuration.

Note that custom configurations doesn't allow you to change what the map looks like, name of the cities and how many cities there are. The rest is all customizable.

If you are interested in only changing the "feel" of the story without changing any of the challenges then you can update the "description.message" for each city and "challenges.display" for each challenge. These are the text items shown to the user.

If you actually want to change the challenges, then you can add/remove/modify the challenges for each city. Here are the fields that are supported for a challenge object:

|field|meaning|
|---|---|
|id| Usually CITY-# format, just has to be unique across the config|
|humanTeamSize| |
|opponentBots| List containing keys to bot ids (either in the bots section or base-bots.json)|
|map|Rocket League arena to play in|
|display|Text to show to the user about this challenge|
|max_score| Optional. If set, it will set the maximum goals mutator so only few values are allowed|
|disabledBoost| Optional. If true, the map is set with no boost so no one will have boost|
|completionConditions| Optional. More details below.|
|limitations|Optional. List that can contain: "half-field". This maps to heatseeker mode|

`completionConditions` is an object. If multiple fields are provided, each condition is "and"-ed 
with the other conditions. A user completes a challenge if all of them are true. The default
condition is that a player must win.

```jsonc
{
  "win": true, //set to false if you don't care,
  "scoreDifference": 3 //player must win by at least this much
  "selfDemoCount": 0 //player must not get demoed more than this
  "demoAchievedCount": 2 //player must at least demo opponents this many time
}
```