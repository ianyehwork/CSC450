## Table of Content
[1. Introduction & Assumptions](#Introduction)  
[2. Context-Aware Features](#Context)  
[3. Brief Implementation Details](#Brief)   
[4. Program Logic](#Program)   
[5. Installation Setup](#Installation)   
[6. Pre-testing Configuration](#Pre)   
[7. Verification Process](#Verification)   

<a name="Introduction"/>

## Introduction & Assumptions
The wake-up manager application (WMA) serves as a context-aware personal agent (CPA) that
helps users automatically set the wake up alarm in the morning based on users’ context. In
addition, the WMA application needs to set appropriate ringer mode (vibrate, loud, or loud and
vibrate) for the alarm according to the user’s surrounding environment. The benefit of having
this application is that 1) users do not need to manually set up alarm which saves time, and 2)
the WMA can figure out the most proper time for the alarm to keep users healthy.
The following assumptions are made in order for the WMA to work properly:
1) The users store the daily events in the Android calendar so that the WMA can retrieve
them.
2) The recommended sleep duration that considered to be healthy is set to be eight hours.
3) The time needed to get to the destination is calculated based on how long it takes to
drive from where user stay for the night.
4) The time needed for user to wake up (including dress-up, having breakfast, etc.) is one
hour. 

<a name="Context"/>

## Context-Aware Features
The WMA needs to capture users’ context in order to set the optimal wake up alarm. The three
main context-aware features to determine the time for the alarm are:
1) The time user falls asleep
2) The time of the first events appears in user’s calendar
3) The time it takes to reach the destination from where user lives

The WMA needs to capture the time users fall asleep but it varies every day so the application
need to capture user’s context in real time to figure out. In addition, the application need to learn
about the time for the event as well as calculate the time needed to get to the destination based
on users’ current location. The information is dynamic so the application need to adapt and
change the time for the alarm in real time.
The WMA also captures the ambient noise around the users when they wake up in order to set
the appropriate ringer mode. This user context is dynamic and unpredictable so the application
need to retrieve this information in real time and make decision based on the result. 

<a name="Brief"/>

## Brief Implementation Details
The brief implementation detail is explained according to the context-aware feature supported
by the proposed application.

To enable the application to capture the time user falls asleep, I decide to use Fence API
(https://developers.google.com/awareness/android-api/fence-api-overview.html) to detect if the
user current activity is still (not moving). In addition, I use the ambient light and noise sensor to
capture environment surrounds user. If the device is not moving within 30 minutes and the
environment is quiet and it is dark, then I can conclude that the user is sleeping.

To enable the application to capture events in user’s calendar, I decide to use Calendar Provider
API (https://developer.android.com/guide/topics/providers/calendar-provider.html) in order to
query and retrieve the information including when the event happens as well as where the event
takes place.

To enable the application to capture the time needed for driving to the destination, I decide to
use Google Distance Matrix API (https://developers.google.com/maps/documentation/distancematrix/) to retrieve the time to reach destination. Moreover, I need to use google play service
(https://developer.android.com/training/location/retrieve-current.html) to get the location users
stay for the night.

To enable the application to set the alarm for user to wake up, I decide to use “AlarmManager”
class in Android API to set alarm for the user (https://developer.android.com/reference/android/
app/AlarmManager.html).

To enable the application to vibrate when the alarm happens, I decide to use “Vibrator” class in
Android API to vibrate the phone (https://developer.android.com/reference/android/os/
Vibrator.html).

To enable the application to adjust alarm’s volume, I decide to use “Audio Manager” class in
Android API (https://developer.android.com/reference/android/media/AudioManager.html).

To enable the application to play the ringtone, I decide to user “RingtoneManager” class (https://
developer.android.com/reference/android/media/RingtoneManager.html) and “Ringtone” class
(https://developer.android.com/reference/android/media/Ringtone.html) in Android API.

To enable the application to use the sensor, I refers to the method used when I developed
project 1 (Android + Sensors) for this course. 

<a name="Program"/>

## Program Logic
The program should detect the time users fall asleep using Fence API, light sensor and noise
sensor. As soon as the time is capture then the application retrieve information from android
calendar about the first event (location and start time) occurs in the following morning.
Meanwhile the current location for the user is retrieved using google play service. Afterwards,
the time required for reaching the destination of the event is calculated.

The following equation is evaluated to determine the time for the alarm:

MINIMUM( (the time user sleep + recommended sleep hours (8hr)), (event start time -
driving time - wake up time (1hr))

For example, the application detects that the user sleeps at 11:30 p.m, the first event occurs at
9:00 a.m in the following morning, the time for driving is 45 minutes. Then we can calculate the
following:
1) the time user sleep + recommended sleep hours (8hr) = 7:30 am
2) event start time - driving time - wake up time (1hr) = 9:00 a.m - 45mins - 1hr = 7:15 a.m

The minimum of (7:30a.m, 7:15a.m) = 7:15 a.m. Therefore we need to set the alarm time to be
7:15 a.m.

<a name="Installation"/>

## Installation Setup
1) Download the required android project in GitHub repository (ZIP File):
a) Context Aware Application
b) Context Middleware
2) Unzip the downloaded zipped file.
3) Open android studio and go to “File” -> “Open”. Select the android studio project for context
aware application and context middleware.
4) Make sure to run the context middleware first and then context aware application. The
context aware application should automatically connect to the middleware after it launches.

Note: When you FIRST run context aware application it will crash. Learn how to resolve
this problem in the next section “Pre testing configuration”.

<a name="Pre"/>

## Pre-testing Configuration
The application compiles fine but fails in running because the permission for the application to
access several services need to be enable on the android device.

Go to Setting -> Apps -> CSC450Project2 -> Permissions, enable all the permissions for the
application (Calendar, Location, Microphone and Storage).

Relaunch the application from android studio. The application should launch successfully and
the user interface should be displayed without any problem.

The following configurations need to be performed before the verification process to avoid
unexpected behavior:
1) Turn off device auto rotation: Go to Setting->Device menu -> Display -> When device is
rotated -> Set to be stay in portrait view.
2) Make sure the android device is connected to the Internet (Wi-Fi or data plan).
3) Setting up the google account in the android device so that the application can have access
to user’s Google calendar. Go to Setting->Accounts->Add account->Google. Enter the
following information as prompted. Email: sampleusercsc450@gmail.com Password:
samplecsc450. Accept the term of service. Tap into the account you have just created then
make sure that the calendar option is enabled. In order to add events to the calendar, go
to Calendar on android device and you should be able to add the event. 

<a name="Verification"/>

## Verification Process
1) User Interface
a) First line displays the ambient noise around the android device.
b) Second line displays the light intensity captured by light sensor on the android device.
c) Third line displays the user’s current location.
d) Fourth line displays count down for activity detection.
e) Fifth line displays the result for the activity detection.
f) Six line displays the count down for the alarm if created.
g) Button “stop alarm” stops the ringtone played by android device when alarm happens.
h) Button “test version” set the recommended sleep duration to be 15 seconds (default is 8
hours and you will have to wait for a long time for the alarm to happen).
i) Button “restart detect” restarts the activity detection after the alarm happens.

2) Calendar events
a) In order to add events to the calendar for the application to read, go to Calendar on the
android device and edit events for sampleusercsc450@gmail.com.
b) The application will retrieve and analyze the events from the calendar ONLY for the
following day (If you test on Nov.13 2016 then the events need to be added to happen
on Nov.14 2016 for the application to capture).

3) Test version
a) Hit the “Test Version” button BEFORE the alarm is set will make the recommended
sleep duration to be 15 seconds. If you want to restore the recommended sleep duration
to 8 hours, you have to restart the application.

4) General Program flow
a) As the program starts, there will be 5 seconds delay before the start of activity detection
count down.
b) After 30 seconds count down the user context (location, ambient noise and light
intensity) is captured and the following decision is made:
i) If user location does not change AND light intensity < 10lux AND ambient noise <
20000, then the alarm is created.
ii) Otherwise the alarm will not be created and the reasoning is displayed.
c) When the alarm is created, the countdown will be displayed on screen and when the
alarm fires, one of the option is chosen according to user context:
i) If user does not have any event tomorrow and ambient noise < 20000, then the
ringer volume is set to be normal and vibrate for 10 seconds.
ii) If user does not have any event tomorrow and ambient noise > 20000, then the
ringer volume is set to be loud.
iii) If the user has event tomorrow and ambient noise < 20000, then the ringer volume is
set to be loud.
iv) If the user has event tomorrow and ambient noise > 20000, then the ringer volume is
set to be loud and vibrate for 10 seconds.
d) When the android device ringing, you can hit “stop alarm” button to stop the ringtone.
e) After you stop the ringtone, you can hit “restart detect” to restart the process discussed
in b).
5) To make context aware application receives callbacks from context middleware, you should
press the button “CONNECT TO CONTEXT MIDDLEWARE.” On the first line.
6) Instead of having the real time light intensity, it will only display whether the light intensity is
less than 10lux. This information is provided by context middleware. You can try to cover the
phone with your hand and then remove it.
7) After connect to the context middleware, the location in latitude and longitude will only be
displayed when location changed is detected by context middleware. So please try to walk
around with the phone in order to see the corresponding location change.
8) When the context middleware triggers callback for light intensity, you can find the similar
message provided below from android monitor log cat:
12-07 04:00:17.306 4198-4212/com.ncsu.tyeh3.csc450project2 D/Light < 10 lux? true
12-07 04:00:18.111 4198-4212/com.ncsu.tyeh3.csc450project2 D/Light < 10 lux? false
9) When the context middleware triggers callback for location, you can find the similar
message provided below from android monitor log cat:
12-07 04:01:49.443 4198-4289/com.ncsu.tyeh3.csc450project2 D/Location Update: 35.77196466, -
78.6742122
12-07 04:01:54.505 4198-4289/com.ncsu.tyeh3.csc450project2 D/Location Update: 35.77200704, -
78.67411335
