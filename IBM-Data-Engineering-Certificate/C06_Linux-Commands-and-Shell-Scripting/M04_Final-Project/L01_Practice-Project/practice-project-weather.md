::page{title="Practice Project: Historical Weather Forecast Comparison to Actuals"}

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-LX0117EN-SkillsNetwork/images/IDSN-logo.png" width="200"/> <br>

Estimated time needed: **30** minutes

## Learning objectives

In this practice project, you will:
- Initialize your log file
- Write a Bash script to download, extract, and load raw data into a report
- Add some basic analytics to your report
- Schedule your report to update daily
- Measure and report on historical forecasting accuracy

We\'ve broken this project down into manageable steps. Feel free to try any or all of these on your own; however, we recommend checking your work with the details provided.

::page{title="Exercise 1 - Initialize your weather report log file"}

### 1.1 Create a text file called `rx_poc.log`
`rx_poc.log` will be your POC weather report log file, or a text file which contains a growing history of the daily weather data you will scrape. Each entry in the log file corresponds to a row as in **Table 1**.

<details>
<summary>Click here for Hint</summary>

> Use the `touch` command or open a new text file from the GUI.

</details>

<details>
<summary>Click here for Solution</summary>
	
> Create `rx_poc.log` by entering the following in your terminal:
	
```
touch rx_poc.log
```

</details>

### 1.2 Add a header to your weather report
Your header should consist of the column names from **Table 1**, delimited by tabs.  
Write the header to your weather report.

<details>
<summary>Click here for Hint</summary>

> Use the `e&#8203;cho` command with the `-e` option, and include tab separators `\t` in a string of names.  
> Consider why the `-e` option is needed.

</details>

<details>
<summary>Click here for Solution</summary>

> Use a shell variable and command substitution:
```
header=$(echo -e "year\tmonth\tday\tobs_temp\tfc_temp")
echo "$header">rx_poc.log
```
> **OR** more directly, use `e&#8203;cho` and redirection:
```
echo -e "year\tmonth\tday\tobs_temp\tfc_temp">rx_poc.log
```
</details>

> **Tip**: Although it might seem redundant, it is better practice to use variables in these cases. Variables make for much cleaner code, which is easier to understand and safer to modify by others or even yourself at a later date. Using meaningful names for your variables also makes the code more \"self-documenting.\"

::page{title="Exercise 2 - Download the raw weather data"}

### 2.1. Create a text file called `rx_poc.sh` and make it an executable Bash script

<details>
<summary>Click here for Hint 1</summary>

> Include a shebang.

</details>

<details>
<summary>Click here for Hint 2</summary>

> Use the `c&#8203;hmod` command.

</details>

<details>
<summary>Click here for Solution 1 </summary>

> Create the file `rx_poc.sh`
```
touch rx_poc.sh
```
>	Include the Bash shebang on the first line of `rx_poc.sh`:
```
#! /bin/bash
```
</details>

<details>
<summary>Click here for Solution 2 </summary>

> Make your script executable by running the following in the terminal:
```
chmod u+x rx_poc.sh
```

> Verify your changes using the `l&#8203;s` command with the `-l` option.
</details>

### 2.2. Assign the city name to `Casablanca` for accessing the weather report

<details>
<summary>Click here for Hint </summary>

> Use the assignment operator

</details>

<details>

<summary>Click here for Solution</summary>

```
city=Casablanca
```
</details>

#### 2.3  Obtain the weather information for Casablanca

<details>
<summary>Click here for Hint</summary>
	
> Use the `curl` command with the `--output` option.  Saved the output to a  file named `weather_report`.
	
</details>

<details>
<summary>Click here for Solution</summary>

> Edit `rx_poc.sh` to include:
```
curl -s wttr.in/$city?T --output weather_report
```

</details>

::page{title="Exercise 3 - Extract and load the required data"}

### 3.1. Edit `rx_poc.sh` to extract the required data from the raw data file and assign them to variables `obs_temp` and `fc_temp`

Extracting the required data is a process that will take some trial and error until you get it right. Study the weather report you obtained in Step 2.3, determine what you need to extract, and look for patterns.

You are looking for ways to \'chip away\' at the weather report by:
- Using shell commands to extract only the data you need (the **signal**)
- Filtering everything else out (the **noise**)
- Combining your filters into a pipeline (recall the use of **pipes** to chain **filters** together)

<details>
<summary>Click here for a Hint to get started</summary>

> Extract only those lines that contain temperatures from the weather report, and save the result to variables representing the temparature output.

</details>

 
#### 3.1.1. Extract the current temperature, and store it in a shell variable called `obs_temp`
Remember to validate your results.

You may have noticed by now that the temperature values extracted from *wttr.in* are surrounded by special formatting characters. These \"hidden\" characters cause the numbers to display in specific color - for example, when you use the `c&#8203;at` command to display your log file.

Unfortunately you cannot perform arithmetic calculations on such formatted text, so you will need to extract the values from the surrounding formatting so you can make use of them later in this lab. 

<details>
<summary>Click here for Hint 1</summary>

> Which line is the current temperature on?

</details>

<details>
<summary>Click here for Hint 2</summary>

> Are there any characters you can use as a delimiter to appropriately parse the line into fields?

</details>

<details>

<summary>Click here for Solution</summary>

> While adding the following lines to `rx_poc.sh`, ensure you understand what each filter accomplishes by using the command line. Try adding one filter at a time to see what the outcome is as you develop the pipeline.

```
#To extract Current Temperature
obs_temp=$(curl -s wttr.in/$city?T | grep -m 1 '°.' | grep -Eo -e '-?[[:digit:]].*')
echo "The current Temperature of $city: $obs_temp"
```

- The first line uses the curl command to fetch weather information from wttr.in for the specified city ($city). It then uses a combination of grep and grep -Eo to extract the current temperature in degrees Celsius and assigns it to the variable obs_temp.

- The second line (`e&#8203;cho $obs_temp`) prints the current temperature to the console.

</details>

#### 3.1.2. Extract tomorrow\'s temperature forecast for noon, and store it in a shell variable called `fc_temp`

<details>
<summary>Click here for Hint</summary>

> Provided you understand the previous pipeline, you will be able to solve this problem through experimentation.

</details>

<details>
<summary>Click here for Solution</summary>

> Add to `rx_poc.sh`:
```
# To extract the forecast tempearature for noon tomorrow
fc_temp=$(curl -s wttr.in/$city?T | head -23 | tail -1 | grep '°.' | cut -d 'C' -f2 | grep -Eo -e '-?[[:digit:]].*')
echo "The forecasted temperature for noon tomorrow for $city : $fc_temp C"
```

- The first line uses a combination of head and tail to narrow down to the line containing tomorrow's noon forecast temperature. grep and cut are employed to isolate and format the temperature information, and, the numeric part of the temperature is captured using grep -Eo.

- The second line (echo $fc_temp) prints the forecast temperature for noon tomorrow to the console.

</details>

### 3.2. Store the current day, month, and year in corresponding shell variables

<details>
<summary>Click here for Hint</summary>

> Use command substitution and the `date` command with the correct formatting options.
> The time zone for Casablanca is UTC+1. To get the local time for Casablanca, you can set the time-zone environment variable, `TZ`, as follows:
``` 
TZ='Morocco/Casablanca'
```
</details>

<details>
<summary>Click here for Solution</summary>
	
> Add to `rx_poc.sh`:

```
#Assign Country and City to variable TZ
TZ='Morocco/Casablanca'

# Use command substitution to store the current day, month, and year in corresponding shell variables:
day=$(TZ='Morocco/Casablanca' date -u +%d) 
month=$(TZ='Morocco/Casablanca' date +%m)
year=$(TZ='Morocco/Casablanca' date +%Y)
```
>**Note**: You might be wondering why we didn\'t just set `hour` to a value of 12, since we want to get the time at noon.
> However, if we did, we would lose the ability to verify that we are actually running the code at the correct *local* time. 
> You should think of the local time as a *measurement* rather than a set number.
	
</details>

### 3.3. Merge the fields into a tab-delimited record, corresponding to a single row in **Table 1**
Append the resulting record as a row of data to your weather log file.

<details>
<summary>Click here for Hint</summary>

> How did you create the header to initialize your log file?

</details>

<details>

<summary>Click here for Solution</summary>
	
> Add to `rx_poc.sh`:
```
record=$(echo -e "$year\t$month\t$day\t$obs_temp\t$fc_temp C")
echo "$record">>rx_poc.log
```

</details>

::page{title="Exercise 4 - Schedule your Bash script `rx_poc.sh` to run every day at noon local time"}

### 4.1. Determine what time of day to run your script
Recall that you want to load the weather data coresponding to noon, local time, in Casablanca every day.

First, check the time difference between your system\'s default time zone and UTC.

<details>
<summary>Click here for Hint 1</summary>

> Use the `date` command twice with appropriate options; once to get your system time, and once to get UTC.

</details>

<details>

<summary>Click here for Solution</summary>

> Running the following commands gives you the info you need to get the time difference between your system and UTC. 
```
$ date
Mon Feb 13 11:28:12 EST 2023
$ date -u
Mon Feb 13 16:28:16 UTC 2023
```
> Now you can determine how many hours difference there are between your system\'s local time and that of Casablanca.
In this example, the local time (11:28) is 5 hours behind UTC (16:28). Therefore, the system\'s time zone (EST) has an offset of UTC-5.

>Now, calculate the time difference between your system and Casablanca:
Casablanca is at UTC+1.
Your system is at UTC-5.
The time difference is calculated as: (Casablanca UTC offset) - (Your System UTC offset).
This is: (+1) - (-5) = 6 hours.
This means Casablanca time is 6 hours ahead of your system\'s time.
Thus, to run your script at noon Casablanca time (12:00 UTC+1), you need to run it at 6:00 AM your system time (EST).
12:00 in Casablanca minus 6 hours = 06:00 (6 AM) on your system.
</details>

### 4.2 Create a cron job that runs your script 

<details>
<summary>Click here for Hint</summary>

> Edit the crontab file and review the crontab syntax description included in the file.

</details>

<details>
<summary>Click here for Solution</summary>

>Edit the crontab file:
>```
>crontab -e
>```
>Add the following line at the end of the file:
>```
>0 6 * * * /home/project/rx_poc.sh
>```
>Save the file and quit the editor.

</details>

### 4.3 Full solution
For reference, here is a Bash script that creates the raw weather report. Try to follow all the steps on your own before looking!

<details>
<summary>Click here for Full Solution</summary>

```
#! /bin/bash
 
#Assign city name as Casablanca
city=Casablanca

#Obtain the weather report for Casablanca
curl -s wttr.in/$city?T --output weather_report

#To extract Current Temperature
obs_temp=$(curl -s wttr.in/$city?T | grep -m 1 '°.' | grep -Eo -e '-?[[:digit:]].*')
echo "The current Temperature of $city: $obs_temp"

# To extract the forecast tempearature for noon tomorrow
fc_temp=$(curl -s wttr.in/$city?T | head -23 | tail -1 | grep '°.' | cut -d 'C' -f2 | grep -Eo -e '-?[[:digit:]].*')
echo "The forecasted temperature for noon tomorrow for $city : $fc_temp C"

#Assign Country and City to variable TZ
TZ='Morocco/Casablanca'

# Use command substitution to store the current day, month, and year in corresponding shell variables:
day=$(TZ='Morocco/Casablanca' date -u +%d)
month=$(TZ='Morocco/Casablanca' date +%m)
year=$(TZ='Morocco/Casablanca' date +%Y)

# Log the weather
record=$(echo -e "$year\t$month\t$day\t$obs_temp\t$fc_temp C")
echo "$record">>rx_poc.log

```

</details>

::page{title="Exercise 5 - Create a script to report historical forecasting accuracy"}

Now that you\'ve created an ETL shell script to gather weather data into a report, let\'s create another  script to measure and report the accuracy of the forecasted temperatures against the actuals.

- To start, create a tab-delimited file named `historical_fc_accuracy.tsv`. 

Insert the following code into the file to include a header with column names:

```
echo -e "year\tmonth\tday\tobs_temp\tfc_temp\taccuracy\taccuracy_range" > historical_fc_accuracy.tsv
```

One key difference between this report and the previous report you generated is that the forecast temperature will now be aligned with the date the forecast is for. As a result, the date will be in the same row as the observed temperature for that date, rather than the previous row on the day that the forecast was made.

<br>

- Also create an executable Bash script called `fc_accuracy.sh`.

Rather than scheduling your new script to run periodically, think of it as a tool you can use to generate the historical forecast accuracy on demand.

### 5.1. Determine the difference between today\'s forecasted and actual temperatures
Rather than writing the script to process all of the data at once, let\'s simplify by solving the problem for just one instance. Later you can modify the script to handle the general case of mupltiple days.

#### 5.1.1. Extract the forecasted and observed temperatures for today and store them in variables

<details>
<summary>Click here for Hint 1</summary>

> Look up the record from yesterday.

</details>

<details>

<summary>Click here for Hint 2</summary>

> Extract the forecast from the appropriate field.

</details>

<details>
<summary>Click here for Solution</summary>

```
yesterday_fc=$(tail -2 rx_poc.log | head -1 | cut -d $'\t' -f5)
```
</details>

#### 5.1.2. Calculate the forecast accuracy

<details>
<summary>Click here for Hint </summary>

> First extract today\'s observed temperature. Then calculate the difference between the forecasted and observed temperatures.
</details>

<details>
<summary>Click here for Solution</summary>

```
today_temp=$(tail -1 rx_poc.log | cut -d $'\t' -f4)
accuracy=$(($yesterday_fc-$today_temp))
echo "accuracy is $accuracy"
```
</details>

> **Tip**: Your weather report must have at least two days of data for this calulation to make sense.
> To test your code you can simply append some artificial data to your weather report, `rx_poc.log`.

### 5.2. Assign a label to each forecast based on its accuracy
Let\'s set the accuracy labels according to the range that the accuracy fits most tightly within, according to the following table. Validate your result.

| accuracy range | accuracy label |
| ------------   | -------------- |
|  +/- 1 deg     | excellent      |
|  +/- 2 deg     | good           |
|  +/- 3 deg     | fair           |
|  +/- 4 deg     | poor           |

<details>
<summary>Click here for Hint 1</summary>

> Use two conditions to compare the accuracy sizes to each positive and negative integer range, accordingly.

</details>

<details>
<summary>Click here for Solution</summary>

```
if [ -1 -le $accuracy ] && [ $accuracy -le 1 ]
then
   accuracy_range=excellent
elif [ -2 -le $accuracy ] && [ $accuracy -le 2 ]
then
    accuracy_range=good
elif [ -3 -le $accuracy ] && [ $accuracy -le 3 ]
then
    accuracy_range=fair
else
    accuracy_range=poor
fi

echo "Forecast accuracy is $accuracy"
```
</details>

### 5.3. Append a record to your historical forecast accuracy file.

<details>
<summary>Click here for Hint</summary>

> Extract the right row and remaining data you need to populate all fields.

</details>

<details>
<summary>Click here for Solution</summary>

```
row=$(tail -1 rx_poc.log)
year=$( echo $row | cut -d " " -f1)
month=$( echo $row | cut -d " " -f2)
day=$( echo $row | cut -d " " -f3)
echo -e "$year\t$month\t$day\t$today_temp\t$yesterday_fc\t$accuracy\t$accuracy_range" >> historical_fc_accuracy.tsv
```

</details>

### 5.4. Full solution for handling a single day
Below is the final script of `fc_accuracy.sh` for handling the accuracy calculations based on just one instance, or day. 

<details>
	<summary>Click here for Solution</summary>

```
#! /bin/bash

yesterday_fc=$(tail -2 rx_poc.log | head -1 | cut -d " " -f5)
today_temp=$(tail -1 rx_poc.log | cut -d $'	' -f4)
accuracy=$(($yesterday_fc-$today_temp))

echo "accuracy is $accuracy"

if [ -1 -le $accuracy ] && [ $accuracy -le 1 ]
then
           accuracy_range=excellent
elif [ -2 -le $accuracy ] && [ $accuracy -le 2 ]
   then
               accuracy_range=good
       elif [ -3 -le $accuracy ] && [ $accuracy -le 3 ]
       then
                   accuracy_range=fair
           else
                       accuracy_range=poor
fi

echo "Forecast accuracy is $accuracy_range"

row=$(tail -1 rx_poc.log)
year=$( echo $row | cut -d " " -f1)
month=$( echo $row | cut -d " " -f2)
day=$( echo $row | cut -d " " -f3)
echo -e "$year\t$month\t$day\t$today_temp\t$yesterday_fc\t$accuracy\t$accuracy_range" >> historical_fc_accuracy.tsv
```
</details>

### 5.5. Generalization to all days
We leave it as an exercise for you to generalize your code to create the entire weather accuracy history. In the next exercise, you will download and work with a synthetic version of this weather accuracy history report.

Here are some suggestions to guide you should you wish to create the weather accuracy history yourself:

- Iterate through your weather log file using a for loop. On each iteration:
  - Use `head` and `tail` to extract consecutive pairs of lines on each iteration
    - This provides you with the current and previous day\'s data
  -  Treat this pair of lines like you did in your code as yesterday and today\'s data
  -  Perform your accuracy calulations as before
  -  Use the correct row to extract date information
  -  Append your resulting data to your historical forecast accuracy report

::page{title="Exercise 6 - Create a script to report weekly statistics of historical forecasting accuracy"}

In this exercise, you will download a synthetic historical forecasting accuracy report and calculate some basic statistics based on the latest week of data. 

Begin by creating an executable bash script called `weekly_stats.sh`. <br>

### 6.1. Download the synthetic historical forecasting accuracy dataset
Run the following command in the terminal to download the dataset to your current working directory.
```
wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-LX0117EN-Coursera/labs/synthetic_historical_fc_accuracy.tsv
```

### 6.2. Load the historical accuracies into an array covering the last week of data
Remember to make your script executable. Also validate your result by printing the array to the terminal.

<details>
<summary>Click here for Hint1</summary>

> First extract the last week of data.

</details>

<details>
<summary>Click here for Hint2</summary>

> Store the values in a temporary file called `scratch.txt`. Write the contents of `scratch.txt` into an array called `week_fc`.

</details>

<details>
<summary>Click here for Solution</summary>
Your shell script should look something like the following. There are many ways to accomplish this task, so the solution provided is not unique.

```
#!/bin/bash

echo $(tail -7 synthetic_historical_fc_accuracy.tsv  | cut -f6) > scratch.txt

week_fc=($(echo $(cat scratch.txt)))

# validate result:
for i in {0..6}; do
    echo ${week_fc[$i]}
done
```

</details>

### 6.3. Display the minimum and maximum absolute forecasting errors for the week

Now use your array to calculate the minimum and maximum absolute errors over the last week. For example, if you have a vaule of -1, change it to be 1. Echo the minimum and maximum absolute errors to the terminal.

<details>
<summary>Click here for Hint1</summary>

> Check for any negative values in your array, and reassign these array entries with their positve counterparts.

</details>

<details>
<summary>Click here for Hint2</summary>

> Initialize two variables, `minimum` and `maximum`. Loop over the array values and modify these two variables as required.

</details>

<details>
<summary>Click here for Solution</summary>

Your final shell script for `weekly_stats.sh` should now look something like the following.

```
#!/bin/bash

echo $(tail -7 synthetic_historical_fc_accuracy.tsv  | cut -f6) > scratch.txt

week_fc=($(echo $(cat scratch.txt)))

# validate result:
for i in {0..6}; do
    echo ${week_fc[$i]}
done

for i in {0..6}; do
  if [[ ${week_fc[$i]} -lt 0 ]]
  then
    week_fc[$i]=$(((-1)*week_fc[$i]))
  fi
  # validate result:
  echo ${week_fc[$i]}
done

minimum=${week_fc[1]}
maximum=${week_fc[1]}
for item in ${week_fc[@]}; do
   if [[ $minimum -gt $item ]]
   then
     minimum=$item
   fi
   if [[ $maximum -lt $item ]]
   then
     maximum=$item
   fi
done

echo "minimum ebsolute error = $minimum"
echo "maximum absolute error = $maximum"

```

</details>

::page{title="Summary"}

Congratulations! You\'ve just completed a challenging, real-world practice project using many of the concepts you\'ve learned from this course. The knowledge you\'ve gained has prepared you to solve many practical real world problems. You\'re almost finished with this course now, and the final step in your journey is to complete the peer-reviewed Final Project.

In this lab, you learned how to:
- Initialize your weather report log file
- Write a Bash script that downloads the raw weather data, and extracts and loads the required data
- Schedule your Bash script `rx_poc.sh` to run every day at noon local time
- Apply advanced Bash scripting to produce reporting metrics
- Create a script to report historical forecasting accuracy
- Create a script to report the minimum and maximum absolute errors for the week

## Authors
Jeff Grossman

### Other Contributors
Rav Ahuja

<!-- ## Change Log

| Date (YYYY-MM-DD) | Version | Changed By        | Change Description                    |
| ----------------- | ------- | ----------------- | ------------------------------------- |
| 202-03-10        | 3.1    | Rajashree Patil    |  Name]Fixed unquoted variable in echo statements in Exercises 1.2, 3.3, and 4.3 to prevent word splitting |
| 2025-09-09        | 3.0     | Rajashree Patil    | Corrected exercise 4.1 solution |
| 2025-01-17        | 2.9     | Nikesh Kumar    | removed the "Hour" column and also in the other places where this is mentioned |
| 2024-04-24        | 2.8     | K Sundararajan    | Minor update in Exercise 3, Step 3.2 based on Sanity testing results |
| 2023-11-30        | 2.7     | K Sundararajan    | Updated instructions based on changes in the output data from wttr.in for obtaining the correct output  |
| 2023-11-04        | 2.6     | K Sundararajan    | `rx_poc.sh` codes further updated to get correct output from `wttr.in`   |
| 2023-10-31        | 2.5     | K Sundararajan    | `rx_poc.sh` codes updated due to changed weather output pattern from `wttr.in`   |
| 2023-08-28        | 2.4     | K Sundararajan    | Code updated in instructions  |
| 2023-08-21        | 2.3     | K Sundararajan    | `rx_poc.sh` code updated for getting correct output  |
| 2023-06-07        | 2.2     | Jeff Grossman     | add minor clarifications              |
| 2023-05-17        | 2.1     | Nick Yi           | ID Review                             |
| 2023-05-17        | 2.0     | Jeff Grossman     | add analytics content, simplify solutions, handle parsing glitches |
| 2023-04-27        | 1.2     | Nick Yi           | QA Pass                               |
| 2023-04-24        | 1.1     | Nick Yi           | ID Review                             |
| 2023-02-14        | 1.0     | Jeff Grossman     | Create initial version of the project | -->

<h3 align="center"> &#169; IBM Corporation. All rights reserved. <h3/>
