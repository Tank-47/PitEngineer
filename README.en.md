# PitEngineer

[简体中文](README.md) | [English](README.en.md)

PitEngineer is an AI race engineer for iRacing, designed around simple controls and brief radio replies. Ask about the race while keeping your attention on driving.

Hold the push-to-talk key to ask a question, then release it and wait for a reply. You can also type a question or use a separate report key to request a race summary.

> The sections below describe the current query scope and example questions. They do not mean that every car, track, accent, or model service combination has been validated in live races.

## Quick start

1. Install and launch PitEngineer.
2. Select your microphone, speech language, speech engine, and hotkeys in Settings.
3. For online AI replies, configure your own model service, model name, and API key.
4. Start iRacing and wait for the data connection and local speech models to be ready.
5. Hold the push-to-talk key, speak, then release it and wait for the engineer's reply.

Local models need time to load on first launch. When no online model is configured, the service fails, or the network is unavailable, some straightforward questions can receive local replies. Offline handling is not guaranteed for every wording or complex follow-up.

## What can you ask?

The questions below are examples, not exact phrases you must repeat. Chinese questions are also supported. Whether a reply can include specific numbers depends on the data available from the game, the connection, and whether enough valid observations have been collected.

### Examples during a race

These examples come from user feedback and historical regression cases. Each arrow means asking the next question after receiving a reply; you do not need to say the entire sequence at once. They illustrate intended queries, not confirmed successful live tests or a guarantee that every complex combination will work.

**Chasing the car ahead: check the gap, then the opportunity**

> "What's the gap to the car ahead?" → "Can I catch him?" → "At which corner am I slower than the car ahead?"

Gap, relative pace, and corner losses are different pieces of information. The application needs to check the current gap and valid observations separately. A gap alone is not enough to promise that you can catch another car.

**Watching the car behind: more than just the gap**

> "What's the gap behind?" → "Is he closing on us?" → "At which corner does he have the biggest advantage?"

Follow-ups depend on the recent topic. When switching targets, explicitly saying "the car behind" is clearer than just saying "him".

**Deciding when to pit: more than checking fuel remaining**

> "Can we make the finish on this fuel?" → "Can I do one more lap before pitting?" → "How many litres do we need at the next stop?"

These are separate strategy queries requiring current fuel, valid consumption data, and race progress. Asking how much fuel to add does not perform refuelling.

**Checking the effect of a tyre change: more than asking whether it is raining**

> "Are dry tyres still suitable?" → "Have we reached the dry-to-wet crossover?" → "Has anyone switched tyre type, and how is their pace after the stop?"

Assessing a tyre change requires valid observed lap times. Rain or a single car entering the pits is not enough to establish a benefit.

**Asking about specific opponents: identify the comparison**

> "What is the gap between P1 and P2?"
>
> "Compare the last lap times of P1 and P12." → "And P12—how does that compare with his previous lap?"

These illustrate two-car queries and follow-ups from historical tests. Vehicle identification and references still need improvement. In multiclass races, specify the class. A question with an in-sentence correction, such as "How much faster is first—no, second place than me?", has previously selected the wrong car or failed to resolve the target and is not currently considered reliable usage.

**Car feedback and setup follow-ups do not imply vehicle control**

> "The rear tyres are struggling."
>
> "What is my brake bias?" → "Adjust it for me."

Subjective feedback should receive a brief acknowledgement, without inventing tyre temperature or wear diagnoses or routinely asking a string of questions. Brake bias can be queried, but a request to change it should explain that the driver must make the adjustment manually, without claiming it has been changed.

The following sections list the query scope by feature.

### Fuel and pit stops

| Query | Example questions |
|---|---|
| Current fuel and estimated laps remaining | "How much fuel is left?" "How many laps can we do on this fuel?" |
| Whether fuel is sufficient to finish | "Can we finish on this fuel?" |
| Whether you can stay out for another lap | "Can we stay out one more lap?" |
| Whether a stop is needed now and whether there is evidence to pit immediately | "Should we pit now?" "Do we need to pit this lap?" |
| Pit window | "What is our pit window?" |
| Suggested fuel addition at the next stop | "How much fuel should we add?" |

Fuel range, fuel addition, and pit windows require reliable consumption and race progress data. Complete advice may be unavailable just after connecting, before enough valid laps have been recorded, or when data is missing. Pit advice does not mean pit services have been performed.

### Lap times, target pace, and personal performance

| Query | Example questions |
|---|---|
| Suggested current race pace | "What is my target pace?" |
| Recent lap times compared with the target | "How far off target am I?" |
| Personal sector performance trends | "Which sector have I slowed down in recently?" "Are my sectors getting worse?" |

Target pace and trends require valid historical samples. They do not guarantee an achievable lap time or establish the cause of a slowdown.

### Nearby cars, positions, and catching opponents

| Query | Example questions |
|---|---|
| Your class position | "What is my class position?" |
| Gaps to the cars ahead and behind | "What's the gap ahead?" "What's the gap behind?" |
| Relative pace of nearby cars | "How much faster per lap is the car ahead?" "Is the car behind closing?" |
| Catch estimates based on current gaps and valid pace data | "Can I catch the car ahead?" "Will the car behind catch me?" |
| Corner performance compared with nearby cars | "At which corners am I slower than the car ahead?" "Where is the car behind gaining on me?" |
| Comparison at a specific corner | "How much slower am I than the car ahead at Turn 3?" |

A gap is not a pace difference. Knowing how many seconds separate two cars may not be enough to estimate a catch. Corner comparisons also require records for the relevant track corners and valid samples for both cars. The application should not invent loss locations when records are unavailable.

### Corner reports and track coverage

The current built-in dataset contains **260 iRacing track/layout entries and 2,283 corner and landmark records**. Different layouts at the same circuit count separately. These figures do not represent 260 distinct circuits or imply that every layout has been validated in live races.

When the current layout can be matched and valid observations have been collected for you and the opponent, the application can:

- Report the main corner time losses relative to the car ahead or behind on request.
- Identify differences using corner numbers or known landmarks, rather than only reporting whole-lap pace differences.
- Answer questions about a specific corner, such as "How much slower am I than the car ahead at Turn 3?"
- Proactively report significant corner losses when automatic reports are enabled, data is reliable, and announcement conditions are met. Reports are not issued at every corner or on every lap.

You can also ask "At which corners is the car behind gaining most?" or "Where am I losing time to the car ahead?"

Corner comparisons use observed segment records. They are not predictions for the next lap and do not establish whether a loss comes from braking points, racing lines, or tyres. Corner features are limited when no layout matches, samples are insufficient, or data is invalid. Other queries depend on their own data requirements.

### Specific cars and same-class opponents

You can try identifying cars by class position, car number, or an explicit identity. For example:

- "What was the class leader's last lap?"
- "What is the gap between P2 and me?"
- "What is the gap between P1 and P2?"
- "Who is driving car number 12?"
- "Is the car ahead on dry or wet tyres?"
- "Is third place in our class in the pits?"
- "What was P2's last lap?"

The current scope includes driver names, car numbers, class/overall positions, last lap times, current lap numbers, time and lap gaps to your car, pit status, known dry/wet tyre types, and the gap between two selected cars.

These queries primarily target identifiable cars in your class. Specific-car queries, spoken corrections, and follow-ups are still being improved. In multiclass races, say "the class leader" explicitly rather than assuming "first place" will always be interpreted correctly. Full opponent vehicle telemetry is not available.

### Weather, track surface, and wind

| Query | Example questions |
|---|---|
| Current weather and rainfall | "Is it raining now?" "How heavy is the rain?" |
| Track surface wetness | "How wet is the track?" "What are the track conditions?" |
| Air and track temperatures | "What is the track temperature?" "What is the air temperature?" |
| Wind speed, direction, and conditions that can be determined | "How strong is the wind?" "Which direction is it coming from?" |

Replies use currently available weather data. They do not guarantee forecasts of rain in the next few minutes, when rain will stop, or instantaneous grip at a particular corner.

### Dry tyres, wet tyres, and performance after a change

| Query | Example questions |
|---|---|
| Current dry/wet tyre suitability | "Should we use dry or wet tyres now?" "Should we stay on slicks?" |
| Observed dry/wet tyre pace difference | "How much faster are wet tyres than dry tyres now?" |
| First observed same-class car to change tyre type | "Who switched to wet tyres first?" |
| Observed lap times after a tyre change | "How are the cars that switched to wets performing?" |

Pace comparisons use only valid observations already collected. They do not guarantee performance for every car or future laps. The benefit of a tyre change cannot be determined without enough samples. The application will not interpret "the rear is sliding" as proof of overheating or tyre wear.

### Car setup, top speed, and repairs

| Query | Example questions |
|---|---|
| Brake bias | "What is my brake bias?" |
| Known rear wing setting or angle | "What is my rear wing setting?" |
| Top speed on the current lap, previous lap, or within the current recording | "What was my top speed last lap?" "What is my top speed so far this lap?" |
| Current versus previous lap top speed | "How much higher is this lap's top speed than last lap's?" |
| Available damage and repair status | "Is there any damage that needs repairs?" |
| Available remaining required/optional repair time | "How long is left on required repairs?" "How long will optional repairs take?" |

Available settings vary by car. If the rear wing is exposed only as a setting index, it will not be converted into an angle. Values from setup files should not be treated as confirmed live adjustments. Top-speed records cover only what the application has recorded. Repair timers generally require the car to be in the pits, a relevant repair state, and a valid timer.

This does not mean every SDK field can be queried directly. The model should not guess missing oil temperature, tyre temperature, tyre pressure, or component damage data.

### Session progress, stints, and incident points

| Query | Example questions |
|---|---|
| Current session type and lap number | "What session is this?" "What lap am I on?" |
| Race time or laps remaining | "How much time is left?" "How many laps remain?" |
| Observed laps in the current stint | "How many laps have we done in this stint?" |
| Personal or car-total incident points | "How many incident points do I have?" "What is our car's total incident count?" |
| Remaining margin to a known incident limit | "How many incident points remain before the limit?" |

Remaining laps in a timed race may be an estimate, which the reply should state. Personal incident points and the car's total incident points are different. An unknown first penalty threshold will not be replaced with the disqualification limit. The margin to a limit is not an allowance for safely having more incidents.

## Follow-ups, feedback, and conversation

You can ask brief follow-ups within the same topic, for example:

> "What's the gap ahead?" → "Is he faster than me?" → "What about the car behind?"

The application tries to retain recent vehicle and topic references, while retrieving specific values from current data. After a long pause, a topic change, or an unclear reference, state the target explicitly again.

You can ask "Who are you?" to learn about the engineer's identity. The application is not a general-purpose knowledge chatbot; reliable replies are not guaranteed for questions that cannot be answered from current race information.

You can also report subjective driving feedback, such as "the rear is sliding" or "the car is understeering". This feedback is acknowledged rather than treated as a verified telemetry diagnosis, and should not require answering a string of follow-up questions every time.

Conversation commands such as "repeat that" and "cancel" have handling paths, but different input methods and complex conversations still need testing. Repeat commands have known issues in some paths and are not guaranteed to work in every situation.

## Queries versus actions

Most features provide information or advice: asking how much fuel to add does not refuel the car, and asking about brake bias does not adjust it.

There is also an experimental feature that must be explicitly enabled: preparing dry or wet tyres for the next pit stop. It requires you to enable the feature, give an explicit command, and confirm it again, with valid live game state available. It does not control driving, call an immediate pit stop, or mean a tyre change has already been completed.

### Preparing dry/wet tyres for the next stop

After explicitly enabling this feature in Settings, you can say:

- "Prepare dry tyres for the next stop."
- "Prepare wet tyres for the next stop."

The engineer first asks for confirmation. Only after you explicitly confirm again, with fresh live data and an eligible car/session state, will the application attempt to set the tyre service for the next stop and record the outcome. It will not execute without confirmation, with stale data, or when conditions are unsuitable. Success depends on the actual operation result; a spoken acknowledgement is not proof of execution.

This only prepares dry/wet tyre service for the next stop. It does not immediately change tyres, drive into the pits, or adjust other car settings.

Automatic refuelling, fast repairs, other pit services, steering, throttle, braking, and car setup control are outside this authorisation.

## Ongoing monitoring and automatic reports

When automatic reports are enabled in Settings, the application continually checks available race information and announces changes worth mentioning. You do not need to press push-to-talk or activate the microphone.

**Automatic reports are currently triggered by events; they are not complete race updates delivered every fixed number of minutes or laps.** The application stays quiet when no change meets the conditions. For an immediate summary, press the report hotkey or ask "How is the race going?"

### Events that can be reported proactively

| Type | Changes that may be announced |
|---|---|
| Fuel risk | Fuel becomes critical, or valid data indicates insufficient fuel to finish |
| Catching the car ahead | Valid gap and pace records show significant progress towards the car ahead |
| Pressure from behind | The car behind closes significantly or presents a pace threat worth watching |
| Changes to nearby cars | A meaningful change in the adjacent same-class car ahead or behind |
| Corner losses | Significant time loss relative to the car ahead or behind at matched corners |
| Sector pressure | Sustained or repeated gains by the car behind in official timing sectors |
| Position changes | Confirmed changes in your car's class position, including positions gained/lost and the new position |
| Nearby pit activity | A nearby same-class opponent actually enters the pits, or observations support a pit-window pattern |
| Pit-cycle situation | Estimated net class position changes when enough observations are available |
| Unusual opponent behaviour | Suspected anomalies such as a same-class opponent losing several positions, without directly claiming a crash or mechanical failure |
| Multiclass traffic | Traffic information that can be determined when faster-class cars approach |
| Weather changes | Meaningful transitions such as rain starting, changing surface wetness, or rain stopping |
| Dry/wet tyre pace changes | Valid records show a change in the pace advantage between dry and wet tyres |
| Tyre changes | First observed same-class dry/wet tyre switches and subsequent valid lap-time comparisons |
| Repeated corner issues | Repeated off-tracks at the same corner, wet-condition off-track risks, or repeated ABS activation supported by data |
| Incident-point reminders | A warning when approaching a known incident limit, without promising how many more incidents can safely occur |
| Hazards ahead | Suspected incidents or abnormal vehicles ahead when there is enough evidence, without guaranteeing detection of every incident |

These are events eligible for selection, not a list that will be read in full every time. Content depends on session type, data quality, event importance, and what has already been announced.

### When does it speak, and when does it stay quiet?

- Races support the fuller set of automatic event reports. Qualifying allows only selected weather and safety reminders. Practice sessions currently do not use this event-reporting system.
- Shared announcement cooldowns and duplicate suppression help avoid repeating unchanged situations. A cooldown ending does not necessarily trigger a report.
- Each report selects a limited amount of information rather than reading every telemetry item.
- New automatic reports are deferred while a call or another interaction is busy. Automatic reporting itself does not start recording or speech recognition.
- Yellow flags, pit-lane status, stale data, or insufficient samples can limit relevant items. Conditions vary between reminder types.
- Pit patterns, catch windows, and net positions are estimates based on observations, not knowledge of an opponent team's plan or the final race result.

Automatic reports provide information and advice. They do not automatically adjust the car or perform unauthorised pit actions, and they cannot replace in-game warnings or driver observation.

## Reports and other interface features

- Separate report key: request a current race summary without activating the microphone.
- Automatic reports: monitor race changes and announce events under the scope and conditions above, rather than delivering complete reports at fixed intervals.
- Corner reports: query or receive proactive reports of major corner losses relative to nearby cars when the layout matches and samples are reliable.
- Radio log: review questions and engineer replies.
- Custom hotkeys: configure push-to-talk and report keys separately.
- Chinese/English replies and local speech output.
- Local recording and replay: review data observed by the application. Replay data is not the current live race.

Reports are not guaranteed to cover every event. Speech output, recognition, and complex queries are affected by devices, system load, historical data, and model services.

## Model services and local speech

You can configure Poe, DeepSeek, Qwen, Gemini, Ollama, and compatible custom endpoints. Availability depends on the selected model, endpoint address, service permissions, and current compatibility. Not every advanced feature from every provider is supported.

The current packaging approach uses faster-whisper for local speech recognition, MeloTTS for Chinese speech, and Piper for English speech. Custom engines and voices depend on the specific version and Settings page. Not every model used in development is included in the package.

Local speech does not mean the entire application works offline. Online models require a network connection and your own valid service credentials. The package does not include an API key. Model service fees, subscription quotas, and limits are set by the respective providers.

## Requirements and data handling

- Windows x64 and a local installation of iRacing.
- A microphone and headphones or speakers for voice interaction.
- Time and memory for the initial local model load; wait until the models are ready.
- Enough valid race observations for fuel, pace, trend, and similar analyses.

Speech recognition and speech synthesis run locally. When using an online model, your question and the processed race information needed to answer it are sent to the selected service. Do not include API keys, access tokens, private recordings, or unredacted logs in feedback.

## Known limitations

- The feature scope does not mean every natural-language phrasing will be recognised correctly.
- Specific-car queries, spoken corrections, some follow-ups, and repeat commands have known issues.
- Missing, stale, or insufficient data may prevent an answer or result in an answer with uncertainty.
- Live tyre temperature, tyre wear, and detailed vehicle damage diagnoses are not provided without supporting data.
- Subjective complaints alone will not be treated as proof of tyre condition or the cause of a mechanical fault.
- Steering-wheel buttons, every audio device, and all live racing scenarios have not been confirmed as validated.
- The application provides supporting information and does not replace driver judgement.

## Downloads and feedback

Some issues remain unresolved. A release will follow once they have been fixed.

When reporting an issue, include the application version, language, input method, original question, expected reply, and actual reply. If possible, also include the car, track, and situation in which the issue occurred. Redact sensitive information from screenshots and logs first.

## Project maintenance

PitEngineer is developed and maintained by Tank-47.

The main application source code is currently closed. Open-sourcing may be considered in the future, but the scope and timing have not been decided. Free personal, non-commercial use is currently permitted. Third-party components and separately licensed speech source code retain their own licences. See the [licensing notice](LICENSE); previously granted licences remain unaffected.
