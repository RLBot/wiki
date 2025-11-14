## Announcing the Tournament

Normally tournament organizers will publish some kind of rules document. Google docs has been popular for that so far. Consider including the following:

- Tournament format
- Submission deadline(s)
- How to submit (upload to google drive folder is a popular technique)
- Stream time and twitch channel
- Your intended tick rate (read and understand [Tick Rate](/v5/botmaking/tick-rate))
- Whether or not hive mind style bots are allowed

[Here's](https://docs.google.com/document/d/1sgaRh_KYlAUTdrSmCLFqTPUKhifiOTQiAFEPkHyOfsw/edit) an example document

## Gathering Submissions

### Python Bot Requirements

People generally just send their whole bot folder, and that works well.

### .NET / C# Bot Requirements

Direct people to [Tournament-Submissions](https://github.com/RLBot/RLBotCSharpExample/wiki/Tournament-Submissions), and also encourage them to do [Auto Launching](https://github.com/RLBot/RLBotCSharpExample/wiki/Auto-Launching)

### Java Bot Requirements

Direct people to [Tournament Submissions](https://github.com/RLBot/RLBotJavaExample/wiki/Tournament-Submissions). If they follow those instructions, you'll be given a nice zip file with a README inside it.

Also encourage them to do [this](https://github.com/RLBot/RLBotJavaExample/wiki/Auto-Launching-Java), it'll make your life much easier

### Scratch Bot Requirements

People who program their bots in Scratch should submit their whole folder.

Scratch program (sb3) requirements

- All the tournament code should be in the player 1 sprite.
- Clicking the green flag should be the only thing necessary to start your program.

CFG file requirements

```
headless = True
separate_browsers = True
pretend_blue_team = True
```

## Deadline Management

It's a good idea to set a bot submission deadline at least 24 hours before the start of the tournament stream. This will give you an opportunity to test all the bots.

Encourage early submissions so you can get a head-start on testing, and allow people edit their submission up until the deadline. If you feel confident from your pre-deadline testing that you'll be able to run most of the bots with no issues, consider giving an extension on the deadline--many people will need one.

## Pre-Tournament Bot Testing

You should test all the bots beforehand.

- Do they start successfully?
- Do they have any obvious behavior problems that might be a problem with your setup?
    - Feel free to check with the bot maker about the expected behavior.
- Do they run properly on *both* blue and orange team? Consider running test matches of each bot vs itself.
- Are the C#/Java bots running on different ports? Each C# and Java bot must run on a separate port from the rest of the bots.

## Running Bots

### .NET / C# Bots

- In the ideal case, if they followed [Auto Launching](https://github.com/RLBot/RLBotCSharpExample/wiki/Auto-Launching) successfully, you will only need to load the cfg file like you would for a Python bot.
- Otherwise, you will need to locate the .exe file in the submission and run it manually. The RLBot framework output will hint about this.
- Make sure the bot's port is different from the rest of the bots.

### Java Bots

Prerequisite: You will need to have the Java Runtime Environment (JRE) installed. You are likely to need the latest one (Java 11 at time of writing) in case any of the botmakers compiled with it.

You hopefully received a zip file for each bot. Extract it somewhere and consult the README.

- In the ideal case, if they followed [Auto Launching Java](https://github.com/RLBot/RLBotJavaExample/wiki/Auto-Launching-Java) successfully, you will only need to load the cfg file like you would for a Python bot.
- Otherwise, you will need to locate the .bat file associated with the Java portion of their bot and run it manually. The RLBot framework output will hint about this.
- Make sure the bot's port is different from the rest of the bots.

If you run into any trouble, consult [this video](https://www.youtube.com/watch?v=VHOkWVYlfa0)

### Scratch Bots

All recently-created Scratch bots should auto start with no issues, BUT often the process takes a while. Consider pausing the match until you see something like "received first input from scratch bot" in the console.

If the bot is very old and does not auto run, they may need to do [this](https://www.youtube.com/watch?v=4oK44syo49Y)

## Streaming and Casting

### Co-casters

If you want to invite others to do commentary with you, you'll need to:

- Give them a real-time view of the action so they can comment on things as they happen.
- Get the sound of their voice flowing through your system audio.

A good way to do this is to start a Discord call and share your screen. If you have internet bandwidth or CPU constraints, make sure you share your screen at a low framerate and / or low resolution.

## Diagnostics

### Bot Response Rate

You can hit the \[home\] key to start rendering bot response rates in the upper left corner of Rocket League. You'll see a percentage that indicates how many of the rendered frames in Rocket League received fresh control inputs from a given bot.

- If you have capped your framerate at 60fps, then you should expect to see 100% for all bots if they are performing well. - If your framerate is higher than 60, you will see percentages lower than 100 for some languages, and that's OK.
- If you ever see a percentage above 100, that's a big concern. It may mean that there are multiple bot processes trying to control the same player. Make sure bots from the previous match are truly dead.

## Running the tournament

### Bracket

All our bots are in their own league on Braacket on which you can also create tournaments with most formats. You can also use your own site of preference or own local program which looks more graphically pleasing. But it's recommended to keep your bracket updated during the tournament so people don't lose track.

### Things to do in between games

- Make sure the FPS is capped at the desired number (maybe 120)
- Don't forget to press 'H' when the games start, so the remove the useless HUD
- Try to check twitch/discord in case there is something wrong and do not be afraid to restart a match
- Have fun!
