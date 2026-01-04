---
search:
  boost: 0.5
---

Hello everyone! We're proud to announce that the Psyonix bot API has finally been integrated into RLBot! We hope you're just as excited as we are about this milestone! 🎉

## Details

Check out this diagram to get some intuition for this migration. Everything on the left staying the same or as close to the same as possible, so your bots are expected to keep working with no changes required!

![](/img/psyonix-api-notes/details.png)

## What you can expect from this new version of RLBot (v4)

We're aiming for a backwards compatibility with this first bot API release, so expect your bots to play almost exactly like they used to!

- The renderer now has occlusion, so 3D lines disappear behind objects correctly!
- You can still use state setting, except:
    - Can’t manipulate boost pads
    - Can’t mess with the car’s jump state
    - Can’t provide partial location / velocity, must fully specify
- The GameTickPacket will have all the same data except:
    - It will have non-interpolated values straight from the physics engine, same as RigidBodyTick!
    - Latest ball touch will always be blank for now (fixed)
    - Boost respawn timers don't work yet
- Unfortunately, quick chat is not supported yet (1/2 fixed)
- We only support Soccar, exhibition mode at the moment. (More modes are supported)
- The game data will currently update in memory at 60Hz, ~~regardless of Rocket League's frame rate~~ (anecdotally it seems to be capped at the frame rate). We may be able to tune that later. (We now have 120hz)
- The Psyonix API itself and the code that interfaces with it directly are closed source at the moment.

## What you can look forward to in the future

- No more breaking after updates! Since RLBot no longer relies on DLL injection, RLBot will continue to work after every Rocket League update.
- LAN support is a possibility in the future. With a virtual LAN (VLAN), this could mean being able to play against bots on different machines over the internet, with your friends!
- Support for Linux and macOS is planned! (C++ and C# bots won't work)
- Rumble support is planned. Bots will be able to take part in the mayhem! (Done)
- Unrelated to the API:
    - DomNomNom is working on a team communication system for bots. (Done)
    - Chip is working on advanced pathfinding for bots.

## Regarding Bugs

This is a lot buggier than a normal release--RLBot broke due to a Rocket League patch and we chose to move forward with the official API rather than fix our old injection technique. Sorry for the bugs / missing features!

Please test out your bots and report any bugs to #issues-and-bugs or the GitHub repository's Issues page: https://github.com/RLBot/RLBot/issues. We appreciate all the help we can get!

List of Busted Bots

- NV Derevo, on some computers

### Rollback Instructions

If it's a serious problem for you, you can roll back your Rocket League and use our old 1.14.12 version. Note that while you're on the old Rocket League, you will be unable to play online!

You can do it by clicking steam://nav/console and then entering `download_depot 252950 252951 6062228629972172324`. Then read https://www.reddit.com/r/Steam/comments/611h5e/guide_how_to_download_older_versions_of_a_game_on/ starting with step 7.

*And then*, once you're done with that, you'll need to pin your rlbot version to 1.14.12. If you're using the old GUI, you can accomplish that by changing this line https://github.com/RLBot/RLBotPythonExample/blob/master/requirements.txt#L3 to `rlbot==1.14.12`.

If you're using the new colorful RLBotGUI, you'll need to go find rlbot-requirements.txt in the vicinity of `C:\Users\yourname\AppData\Local\RLBotGUI\rlbot-requirements.txt` and do the change there.

## Thanks

Thank you to everyone for sticking with the community! Of course, a massive thank you to Psyonix for supporting us and giving us the API! We can't wait to see RLBot evolve further!
