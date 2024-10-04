---
layout: post
title: Cleaning JSON data.
---
On Monday I started a new project for Erdös Institute's data visualization minicourse.  For the project I plan to build a dashboard using my Fitbit data for the past year and `d3.js`, a JavaScript library.  It was daunting at first, every time I see JavaScript I get intimidated, even when I watched Matt's `d3.js` videos I had trouble following what was going on.  So I thought by using `d3.js` in my project I could get more comfortable with JavaScript.

Things have moved pretty slowly so far.  The first thing I had to do was pick out what data to use.  Fitbit exports _all_ the data, including features that I didn't even use with my Fitbit subscription.  The files are all there, but not organized in an obvious way.  Some of the files have the data from each month, while others have it for each day.  Some of the files are `.csv` files, and some are `.json` files.  So what I decided to do was create a Jupyter notebook and use python to consolidate the data.  I chose 7 categories:

1. **Exercise.**  Activity (run/walk), activity level minutes (sedentary, lightly, fairly, very), average hr, Calories burned, duration (ms), steps, minutes and Calories burned in hr zones (out of range, fat burn, cardio, peak), total active zone minutes.
2. **Active zone minutes.** Minute-by-minute active zone minutes.  Doubles for cardio and peak.
3. **Weight.** Daily weight and BMI.
4. **Readiness score.** Daily readiness score with subcomponents.
5. **Daily respiratory rate.** Average breaths per minute during deep sleep.
6. **Sleep score.** Overall score with components, minutes in deep sleep, resting hr, percent restlessness.
7. **Sleep stages.** Time spent in light, deep, REM.

I spent a lot of time trying to consolidate all the data in each category and then export it as a `.csv` file.  However, I could never figure out how to export JSON data to `.csv`.  Turns out I didn't have to.  I started rewatching the `d3.js` videos and saw that in my source file I can load JSON data directly.

I relooked at some of Matt's `d3.js` videos to figure out how to load my data onto my webpage.  In Matt's sample html files he always logs the data into the console, so I did that, too.  I loaded the `.json` file for "Active zone minutes" and the `.csv` file for "Weight".  After that I wasn't really sure what to do next.  I went to Matt's office hours Wednesday, hoping he could give me some direction, the main thing he said was that I need to clean my data some first.

I decided to work on the Exercise files first.  Each file had nested dictionaries, and some of the data was redundant.  I started with the first file and just tried to parse each entry.  I was able to make a dictionary with just the entries I wanted: "activityName" (Run/Walk, I excluded Weights, since I didn't record every time I lifted weights), "averageHeartRate", "belowZonesMinutes", "fatBurnMinutes", "cardioMinutes", "peakMinutes", and "startTime".  After that I was able to use list comprehension to make a list of the extracted data, then make a loop to do it for all the files.  I discovered to open a file it was more efficient to use a `with` statement:
```
with open(raw_exercise_files_path+"\\"+file) as file:
  parsed_file = json.load(file)
```
Apparently with the `with` statement I don't need to close each file after I read it.  I didn't know if I could use list comprehension for the big loop, even though I was creating a list.  In this case I was using the `.extend` function and I needed an existing variable for the list of dictionaries.

My resulting list was stored as a list, so I used the command `json.dumps` to change it to a string, then to save it to a `.json` file I used the commands: 
```
save_file = open("CleanedData\\exercise_cleaned.json", "w")
json.dump(json.loads(all_extracted_data_string), save file, indent = 4)
save_file.close()
```
Note the `json.dump` command versus `json.dumps`.  The first one allows me to save the JSON object and the second one converts the list to a JSON string.  I'm not sure my code was the most efficient, since I think `json.dump` undoes the `json.loads` command.  However, the code worked and when I opened the file it was even parsed (the `indent` argument says how far to indent for each object).

My next step was to load my cleaned `.json` file and my weight `.csv` file using `d3.js` and just start playing around with graphing the data.  However, all of my data requires the $x$-axis to be a time scale.  I found some code to try to do this, but it hasn't worked yet.  Matt has more office hours today so I will ask him about it.
