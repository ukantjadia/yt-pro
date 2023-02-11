# this is a roadmap for performing a remix shorts video for 
- get the youtube-dl [ link ](https://github.com/youtube-dl)
- gather the shorts link(id) 
  - what I have done is choose a channel and gather the all shorts link available on that channel. -> do it for multiple channel.
  - I am using the inspect from the browser and applying this code in the console
  - open the youtube channel of which you want to have shorts **open the shorts section of that channel**
  - right click and open the inspect and go to the console tab and run the following as required :->

- 1. 
```javascript
var scroll = setInterval(function(){ window.scrollBy(0, 1000)}, 1000);
```
- 2. for video 
```javascript
window.clearInterval(scroll); console.clear(); urls = $$('a'); urls.forEach(function(v,i,a){if (v.id=="video-title"){console.log('\t'+v.title+'\t'+v.href+'\t')}}) ; 
```
- 2. for shorts
```javascript
window.clearInterval(scroll); console.clear(); urls = $$('a'); urls.forEach(function(v,i,a){{console.log('\t'+v+'\t')}});
```

- After running this in the console you will get the list of so many links.
- Now right-click on the console area and select the `save as` option and give the name of the log file.


This will give some extra links also at the start and end of the file, which you have to trim manually or automate it!!
<br> and also this will also give some duplicate links trim them too. use this to remove duplicate `awk '!seen[$0]++' file.txt > output.txt`
<br> To remove the extra link use this `grep '/shorts/' output.txt > link.txt`
<br> We only want to have the id of link for that do this `cut -d '/' -f5 > output.txt`

- Choose a time stamp for each video  
  - I am choosing a single common time stamp for each short (end_time - start_time = time_duration_of_that_short)
  - to add the time stamp in file do `awk '{print "00:20 " $0;}' out.txt > output.txt` this will add the time stamp as 20 sec

- select the random video using `shuf` from different videos (the time of a single short will be aggregated as the total duration of videos)


- now we will down the small clip according to the time stamp we want 
```
ffmpeg -i $(youtube-dl -f bestvideo --get-url https://youtu.be/iZsG8C7i7So) -ss 00:05 -t 6 -f mp4 out.mp4
```
This will work for the one video 

- To use this on a list of id and time stamp do 
```
while read ts id ; do ffmped -i $(youtube-dl -f bestvideo --get-url https://youtu.be/$id) -ss $ts -t 6 -f mp4 $id.mp4 ; done < output.txt
```

