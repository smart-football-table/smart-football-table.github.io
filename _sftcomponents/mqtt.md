---
title: 'MQTT'
---

Find out about the reason for MQTT and the different messages used.

## Reason for MQTT

For communication between different parts in the system MQTT is used. As seen in the Architecture, nealry every part in
the system publishes/subscibes to a broker. With this setup, messages in different topics can trigger different
reactions. For example, this is used to inform the UI about a new score.

## MQTT messages

| topic                      | Description                                               | Example payload          | Comment    |
|----------------------------|-----------------------------------------------------------|--------------------------|------------|
| leds/backgroundlight/color | Sets the background light, default is #000000             | #CC11DD                  |            |
| ball/position              | The ball absolute position on the table, between 0 and 1  | { "x": 0.5, "y": 0.3333} |            |
| ball/velocity              | The balls average speed in the last half second, km/h     | { "velocity": 30 }       | deprecated |
| ball/velocity/kmh          | The balls average speed in the last half second, km/h     | 30                       |            |
| ball/velocity/ms           | The balls average speed in the last half second, m/s      | 5                        |            |
| team/scored                | The id of the team that scored                            | 1                        |            |
| game/score                 | The teams' scores                                         | { "score": [ 0, 3 ] }    | deprecated |
| game/score/\<teamid\>      | Score of team with teamid \<teamid\> (zero-based)         | 3                        |            |
| game/foul                  | Some foul has happened                                    | -                        |            |
| game/start                 | A match starts                                            | -                        |            |
| game/gameover              | A match ended                                             | { "winners": [ 0 ] }     |            |
| game/idle                  | Is there action on the table                              | true                     |            |
| game/reset                 | Command to interrupt the running game and startover       |                          |            |
| leds/foregroundlight/color | Foreground light overrules everything else if not #000000 | #111111                  |            |
