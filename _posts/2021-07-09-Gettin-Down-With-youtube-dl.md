---
layout: post
title:  "Getting Down with youtube-dl and R"
by: "Jake Stoetzner"
---

## I Am Officially Old

I was perusing the internets the other day, and stumbled across a program called [youtube-dl](https://youtube-dl.org/). Basically, it is a command line interface to download and save videos and audio files from (you guessed it) YouTube.

I have a (strange?) goal to listen to every single album on the [Rolling Stone List of Top 500 Albums](https://www.rollingstone.com/music/music-lists/best-albums-of-all-time-1062063/). I am not the [first person to listen to all of the albums](https://tonedeaf.thebrag.com/meet-music-fan-just-smashed-500-greatest-records-time/) or [even the second person to do so thanks to COVID](https://medium.com/live-your-life-on-purpose/sheltering-in-place-and-listening-to-every-album-on-the-rolling-stone-top-500-list-3d4e83c4918e), but I might be the least focused. To date, I think I have listened to about 50 albums, starting from the bottom up. Lacking motivation, I thought I would inspire myself by buying all 500 albums on CD and listening to them [old school like the Maxwell man](https://bestclassicbands.com/maxell-blown-away-guy-2-3-18/).

![Blown Away](https://bestclassicbands.com/wp-content/uploads/2018/02/Maxwell-Blown-Away-Guy.jpg)

But then I remembered I'm cheap, and I don't want to drop $10k on CDs or $20k on vinyl. I don't understand torrenting, and streaming each album on Spotify lacks a certain "old school cool" that runs counter to the spirit of the RS500.

Enter youtube-dl and the ability to rip a high-quality version of each album. If [downloding music from Limewire was like having unprotected sex with the internet](https://melmagazine.com/en-us/story/an-oral-history-of-limewire-the-little-app-that-changed-the-music-industry-forever), then using youtube-dl is like being in a monogamous, committed relationship. But with YouTube.

## Basics of youtube-dl

Downloading the program is fairly simple. [They have detailed installation instructions](https://ytdl-org.github.io/youtube-dl/download.html) for just about any operating system.

From Mac, make sure you have [homebrew](https://brew.sh/) installed then open a Terminal window and enter

```
$ brew install youtube-dl
```

Or you can use

```
$ sudo curl -L https://yt-dl.org/downloads/latest/youtube-dl -o /usr/local/bin/youtube-dl
```

Once installed, restart the Terminal. You can now download a YouTube video. Say you`re itching for that little ol band from Texas [490th ranked album Tres Hombres](http://www.thers500.com/albums/490-zz-top-tres-hombres-1973). Find the url and enter it on the command line as such:

```
$ youtube-dl https://youtu.be/4BNjy4LRWo8
```
Youtube-dl downloads the video and audio of videos [separately](https://github.com/ytdl-org/youtube-dl#do-i-need-any-other-programs) and then merges the two back together. Since I only want **mp3** versions of each song, I had to download [ffmpeg](https://www.ffmpeg.org/) to help youtube-dl with the process. You can install ffmpeg from the command line with

```
$ brew install ffmpeg
```

Then to download an **mp3** version based on a YouTube url

```
$ youtube-dl --extract-audio --audio-format mp3 <YOUTUBE VIDEO URL OR ID>
```

Youtube-dl is as simple as putting the **youtube-dl + url** format into the CLI. Where it really shines are the options available for customizing the quality, file type, file location and [numerous other options](https://github.com/ytdl-org/youtube-dl#options). For example, assume you want to download all the videos from a playlist in **mp3** in separate files on your computer:

```
# Download YouTube playlist videos in separate directory indexed by video order in a playlist
$ youtube-dl -o '%(playlist)s/%(playlist_index)s - %(title)s.%(ext)s' <YOUTUBE PLAYLIST URL OR ID>

# Download all playlists of YouTube channel/user keeping each playlist in separate directory:
$ youtube-dl -o '%(uploader)s/%(playlist)s/%(playlist_index)s - %(title)s.%(ext)s' <YOUTUBE PLAYLIST URL OR ID>

# Download YouTube playlist videos and convert to mp3 in separate directory indexed by video order in a playlist
$ youtube-dl -x --audio-format mp3 -o '%(playlist)s/%(playlist_index)s - %(title)s.%(ext)s'  <YOUTUBE PLAYLIST URL OR ID>
```

To update youtube-dl you can use homebrew again.

```
$ brew upgrade youtube-dl
```

You don't only have to *only* use YouTube as your source url. In fact, you can download [video and audio from 1000s of sites](https://github.com/ytdl-org/youtube-dl/blob/master/docs/supportedsites.md) all from the command line.

A couple of useful basic guides:

* [ytdl-org/youtube-dl](https://github.com/ytdl-org/youtube-dl#output-template-examples)
* [youtube-dl cheatsheet](https://sachithmuhandiram.medium.com/youtube-dl-cheatsheet-bcc0782e7124)
* [Youtube-dl Tutorial With Examples For Beginners - OSTechNix](https://ostechnix.com/youtube-dl-tutorial-with-examples-for-beginners/)
* [Youtube-dl format code cheatsheet](https://voussoir.net/writing/youtubedl_formats)
* [youtube-dl: Supported sites](https://ytdl-org.github.io/youtube-dl/supportedsites.html)
* [youtube-dl for downloading music from YouTube](https://gist.github.com/ivanskodje/5e6f4f7d883cd1124d6d0680b51c4cd9) - [DB](https://www.dropbox.com/s/31xf8zfd0yvnhzg/youtube-dl%20for%20downloading%20music%20from%20YouTube.pdf?dl=0)

## Building a Data Frame for the RS500 Links

As slick as youtube-dl is, you still need to have the url of the video or playlist to download it.

### Getting the Artist and Album Information

But first, I need access to all of the artist and album names. One option is to use the R package [rvest](https://rvest.tidyverse.org/articles/rvest.html) to scrape the [album and artist name from rollingstone.com](https://www.rollingstone.com/music/music-lists/500-greatest-albums-of-all-time-156826/). This is a little complex because you have to deal with a few pagination and loading issues (discussed further below), because the site divides the list into sets of 100.

You could also scrape the table on [Wikipedia:WikiProject Albums/500 - Wikipedia](https://en.m.wikipedia.org/wiki/Wikipedia:WikiProject_Albums/500) which is what the code does below.

```
library(rvest)
library(tibble)

# - read in html from webpage
webpage <- read_html("https://en.wikipedia.org/wiki/Wikipedia:WikiProject_Albums/500")

# - return a list of tables from the page to review to select the correct one (ie, number 3)
tbls <- html_nodes(webpage, "table")

# - create a list of dataframes of the particular table we want to parse
df.tbls <- webpage %>%
        html_nodes("table") %>%
        .[3] %>%
        html_table(fill = TRUE)

df.tbls <- df.tbls[[1]]  %>%
            select(1:3) %>%
            rename(
              "No" = "#",
              "Album" = "Album",
              "Artist" = "Artist(s)"
            )

# df.tbls            

# A tibble: 500 x 3
      No Album                           Artist                   
   <int> <chr>                           <chr>                    
 1     1 What's Going On                 Marvin Gaye              
 2     2 Pet Sounds                      The Beach Boys           
 3     3 Blue                            Joni Mitchell            
 4     4 Songs in the Key of Life        Stevie Wonder            
 5     5 Abbey Road                      The Beatles              
 6     6 Nevermind                       Nirvana                  
 7     7 Rumours                         Fleetwood Mac            
 8     8 Purple Rain                     Prince and the Revolution
 9     9 Blood on the Tracks             Bob Dylan                
10    10 The Miseducation of Lauryn Hill Lauryn Hill              
# ... with 490 more rows
```
The above R code was taken from a great tutorial [Scraping HTML Tables · Bradley Boehmke](http://bradleyboehmke.github.io/2015/12/scraping-html-tables.html) - [DB](https://www.dropbox.com/s/powy4qztrb6x6nb/Scraping%20HTML%20Tables%20%C2%B7%20Bradley%20Boehmke.pdf?dl=0).

If you don't want to use R, a quick google search found an **xls** [version of the list](https://liveduq-my.sharepoint.com/:x:/g/personal/longr3_duq_edu/EWRasWlT_81ArS8f_AEa8AMBxC28wGf8s25zFefsVcceUw?e=aauPkJ) and a **PDF** [version](https://docdro.id/YIYtXiQ). I also took the time to create a Google Sheet that has all of the info in it. You can [download a copy by clicking here](https://docs.google.com/spreadsheets/d/193tl0rv4_jOHL_Jkjk3w_3OM4yuAAb4pUcmvg2FMgmc/copy).  If you need a copy of it in Excel format, you [can download that here](https://www.dropbox.com/s/ttii61y07ydjcav/rs500_full_list.xlsx?dl=1).

### Getting the YouTube Links

Now that we have the album and artist info, we need to get the YouTube playlist links.  The process will involve finding the YouTube search result url and then extracting the actual YouTube playlist links from there.

[Playlists](https://www.google.com/amp/s/www.adweek.com/performance-marketing/youtube-playlists/amp/) are user or creator collections of videos. From my past manual search experience, you can find the majority of albums on the RS500 list setup as YouTube playlists.

The url structure of a YouTube playlist search is

**(youtube base url) + (search terms separated by "+") + ("%2C+playlist")**

where the youtube base url is "https://www.youtube.com/results?search_query=".

If you wanted to search for the album [Nervmind](http://www.thers500.com/albums/17-nirvana-nevermind-1991) by Nirvana, you could enter:

```
"https://www.youtube.com/results?search_query=" + "Nirvana+Nevermind+Full+Album" + "%2C+playlist"
```

which in R could be constructed by using the ```paste0()``` command:

```
paste0("https://www.youtube.com/results?search_query=","Nirvana+Nevermind+Full+Album","%2C+playlist")
```

Note that adding the term "Full Album" to the search helps return results with full album playlists.

Since we have the Artist and Album info in data frame format, you could use the following R-code to build the playlist search url column:

```
# - load the stingr library
library(stringr)

# - split Album Name and Artist name by Space
album_name = str_split(df.tbls$Album, pattern=" ")
artist_name = str_split(df.tbls$Artist, pattern = " ")

# - add "+" between each term along with "Full+Album"
album_name <- lapply(album_name, function(x)(paste0(x, collapse="+")))
artist_name <- lapply(artist_name, function(x)(paste0(x, collapse="+")))
helper <- rep(paste0("Full","+","Album"), length(album_name))

# - create base_url and playlist command
base_url <- rep("https://www.youtube.com/results?search_query=", length(album_name))
helper2 <- rep("%2C+playlist", length(album_name))

# - compile album_name, artist_name and helper into vector
yt_playlist_search_url <-
  mapply(
    function(a,x,y,z)(
      paste0(
        a,          # base_url
        x,          # artist name
        "+",
        y,          # album name
        "+",
        z           #helper2
      )
    ), base_url, artist_name, album_name, helper2, SIMPLIFY = F
  )

# - add yt_playlist_search_url to full rs500 tibble
yt_playlist_clean <- as_tibble_col(unlist(yt_playlist_search_url))

rs500_full_list <-
  df.tbls %>%
  add_column("YouTubeSearchURL" = yt_playlist_clean$value)

# - save a copy as a csv file
write_csv(rs500_full_list, path = "rs500_full_list.csv")
```

### Scraping the YouTube Search page for Playlist Links

We now have the urls to access the YouTube search results of each of the desired artist/album. The next step is to get the url of the actual playlist to put into youtube-dl. YouTube can be hit or miss with the accuracy and quality of each playlist result; oftentimes a playlist indicating it has the entire album will have the wrong songs, an incorrect version of a song or the album order will be incorrect. I propose 4 separate solutions below to overcome this issue.  Each have their own positives and negatives.

#### 1. Manual Scraping

Manually parsing the search urls and selecting the correct playlist that matches the real album can be important. With the links conveniently at hand, you can make short work of parsing the links by hand.

```
# - use R to open up a browser with the search results for the second url
browseURL(rs500_full_list$YouTubeSearchURL[[2]])

# - use R to open up multiple browser tabs from p.start to p.end
# - repeat as many times as needed
p.start <- 5
p.end <- 10
lapply(rs500_full_list$YouTubeSearchURL[p.start:p.end], function(x)(browseURL(x)))
```

#### 2. Use RSelenium

The ```rvest``` package is wonderful for scraping *static* html websites. For pages that load dynamically through javascript or some other server side script, ```rvest``` does not perform as well. Unfortunately, YouTube is one of those pages that loads variable content in multiple stages. It is challenging to extract any of the search results since they are dynamically loaded.

The ```RSelenium``` package acts as sort of a remote-controlled web browser from R. You input commands in R to control a Chrome, Firefox or phantomjs browser and then navigate each url accordingly.

The main advantage of ```RSelenium```is the ability to scrape dynamically loaded pages.

From [RSelenium Tutorial](http://joshuamccrain.com/tutorials/web_scraping_R_selenium.html):

>Many websites are difficult to scrape because they dynamically pull data from databases using JavaScript and jQuery. For example, on common social media sites such as LinkedIn or Facebook, as you scroll down the page new content is loaded and the URL doesn’t change. These websites are much more difficult to scrape. An easy scraping task is when we can adjust the URL to load a new page based on some systematic pattern.

The code below is a brief overview on loading the first url in the RSelenium browser.

```
library(RSelenium)

# start up local selenium server
rD <- rsDriver()
remDr <- rD$client

# navigate to desired page
remDr$navigate(rs500_full_list$YouTubeSearchURL[[1]])
```

Now extract the url of each of the appropriate playlists from the HTML code. (NOTE: because of the time involved with this solution, I have not completeled the example.  More to come on this issue in future posts.)

#### 3. Use a "pseudo" API

[A github user named Herman Fassett](https://github.com/HermanFassett) has created am [API endpoint that returns YouTube search results in JSON format](https://github.com/HermanFassett/youtube-scrape).

The API call uses a base url combined with the YouTube search term(s). For example, to search for the album Nevermind by Nirvana, you would use:

```
http://youtube-scrape.herokuapp.com/api/search?q=Nirvana%20Nevermind
```

The call returns structured JSON output. You can use the ```httr``` package to make the **GET** call and either the ```RJSONIO``` or the ```jsonlite``` package to parse the results. The following R code calls the API and returns all the results for the RS500 albums using the ```artist_name``` and ```album_name``` lists created above.

```
library(RJSONIO)
library(jsonlite)
library(httr)

# - put together the search terms
base_url_json <-
	"http://youtube-scrape.herokuapp.com/api/search?q="

helper4 <-paste0(artist_name,'+',album_name,'+',helper)

rs500_json <-
	paste0(base_url_json, helper4)

# - create a list to store the JSON data
raw_data_json2 <-
	vector("list", length(rs500_json))

# - loop through all of the data giving 20 seconds between each call
# - note that this will take a while!
for (i in seq_along(rs500_json)){
	raw_data_json2[[i]] = GET(rs500_json2[[i]])
	Sys.sleep(20)
}
```

The code above returns a 500 element list of the search results from YouTube. These results are in JSON format, so they need to be parsed into an R-friendly format.  Because it waits 20 seconds between each call, it takes around 3 hours to run.  I have no idea if this is an appropriate amount of time to wait between calls, but I didn't want to overload the API.  Once we have received the raw data, we can convert it to a more R-friendly format.

```
# - transform the raw data to a list
rs500_json_clean <-
  lapply(raw_data_json2, function(x)(fromJSON(rawToChar(x$content))))

# - save a copy of the data as an RDS file
saveRDS(rs500_json_clean,"rs500_json_clean.rds")

# - extract the playlist id and the playlist url
rs500_json_playlist_id <-
  lapply(rs500_json_clean, function(x)(x$results$playlist$id))

rs500_json_playlist_url <-
  lapply(rs500_json_clean, function(x)(x$results$playlist$url))

# - remove NA values from playlist id and the playlist url
rs500_json_playlist_id <-
  lapply(rs500_json_playlist_id, function(x)(x[!is.na(x)]))

rs500_json_playlist_url <-
  lapply(rs500_json_playlist_url, function(x)(x[!is.na(x)]))
```

#### 4. Use the html version of duckduckgo

Search engines are the most likely to avoid webscraping of results, because it is counter to their business model of having human eyes on the screen.  Dynamically loaded content is the biggest challenge. However, some search engines still output into html including [DuckDuckGo](https://duckduckgo.com) by accessing the HTML version of the site at [https://duckduckgo.com/html/](https://duckduckgo.com/html/).  Fair warning, the code below works but unless you do some work with the header information, including the [user agent request header](https://en.wikipedia.org/wiki/User_agent), it will limit your output to three results before sending a [Forbidden (HTTP 403) code](https://en.wikipedia.org/wiki/HTTP_403).

```
duck_url <- "https://duckduckgo.com/html/?q="

helper3 <- rep("site%3Ayoutube.com", length(album_name))

duck_search_url <-
  mapply(
    function(a,x,y,z)(
      paste0(
        a,          # base_url
        x,          # artist name
        "+",
        y,          # album name
        "+",
        z
      )
    ), duck_url, artist_name, album_name, helper3, SIMPLIFY = F
  )

duck_data <- vector("list", length(duck_search_url[1:10]))

my_user_agent <- "Mozilla/5.0 (Windows NT 5.1; rv:52.0) Gecko/20100101 Firefox/52.0"

for(i in seq_along(duck_data[1:10])){
  duck_data[[i]] <-
    read_html(GET(duck_search_url[[i]],user_agent(my_user_agent))) %>%
    html_elements("a.result__url") %>%
    html_text2()
    Sys.sleep(3)
}

```

#### Summary of Playlist Link Scraping

This was by far the toughest part of the project, and it took the most time.  In the end, the "pseudo" API option worked the best.  But the issue remains whether the playlist links that YouTube returns are the best option for your end goal:  a complete copy of the album.  I would suggest that you take the time to parse the search urls as suggested in Option #1.  The goal of the project is to be more intentional in your listening habits.  This will help with that process!

## Download the albums

A QUICK WORD OF CAUTION - make sure you are aware of the potential ramifications of downloading copyrighted materials from the internet, including YouTube. End sermon.

Now that we have decided on the albums to download, and acquired the appropriate YouTube playlist links, the final step is to use youtube-dl to download them.  But first, we need to decide on how to organize the albums, and as a special extra step, provide ourselves a format to record our thoughts while listening to each one.

### Organization of Albums

There are a **lot** of different opinions on [how to organize your electronic music](https://homedjstudio.com/organize-music-library/).  However, we will keep it simple and use the built-in file naming convention in youtube-dl and a simple directory structure:

```
.
+-- _top-level-directory
|   +-- _artist-name1
|   |   +-- _album-name1
|   |   |   +-- song-name1
|   |   |   +-- song-name2
|   |   +-- _album-name2
|   +-- _artist-name2
|   |   +-- _album-name1
```

youtube-dl allows you to download a playlist and specify the name and location of each file.  The command line code below places it in a directory with the title of the playlist as it appears on YouTube.  The code also names the file with the playlist order and the title of the YouTube video.

```
$ youtube-dl --extract-audio --audio-format mp3 -o '/%(playlist)s/%(playlist_index)s - %(title)s.%(ext)s' <youtube-playlist-id-or-url>

# - specific example
$ youtube-dl --extract-audio --audio-format mp3 -o '/%(playlist)s/%(playlist_index)s - %(title)s.%(ext)s' PLc60gkdW0bcFbUwumMvBpxVrFbkeRzoPf
```

For the top-level directory, name it, check if it already exists and if it doesn't, create it.

```
my.dir <- file.path("INSERT/FILE/PATH/HERE/rs500")
if(!dir.exists(my.dir)){dir.create(my.dir)}
```

Return a list of unique artists on the list (remember that some have multiple albums).  Then create the file path and check to see if a directory already exists in the top-level-directory.  If one does not, have R create one. (Also, thanks for reminding me you can't name a directory "AC/DC").

```
my_rs500_artists <- data.frame("Artist"=unique(rs500_full_list$Artist))

# - rename the files to remove illegal file names and replace with "_"
my_rs500_artists_file_name <- gsub("[^a-zA-Z0-9\\.\\-]","_",my_rs500_artists[[1]])

# - create file paths
my_rs500_artists_file_path <- file.path(paste(my.dir, my_rs500_artists_file_name, sep="/"))
lapply(my_rs500_artists_file_path, function(x)(if(!dir.exists(x)){dir.create(x)}))

# - create a data frame with all info on artists
my_rs500_artists_with_file_path <- data.frame("Artist"=unique(rs500_full_list$Artist),"File.Path"=my_rs500_artists_file_path)

```

Now we can merge the unique artist name with our original full list and with the file path list.

```
my_rs500_merge_list <- merge(my_rs500_artists, rs500_full_list,by="Artist")

# - add in file paths and arrange by ascending order
my_rs500_merge_list_2 <- merge(my_rs500_merge_list, my_rs500_artists_with_file_path, by="Artist") %>% arrange(No)

# - add album folders for each artist
my_rs500_album <- data.frame("Album"=my_rs500_merge_list_2$Album)
my_rs500_album_file_name <- gsub("[^a-zA-Z0-9\\.\\-]","_",my_rs500_album[[1]])
my_rs500_album_file_path <- file.path(paste(my_rs500_merge_list_2$File.Path, my_rs500_album_file_name, sep="/"))
lapply(my_rs500_album_file_path, function(x)(if(!dir.exists(x)){dir.create(x)}))

# - add album path to master list
my_rs500_merge_list_3 <- data.frame(my_rs500_merge_list_2,"Album.File.Path"=my_rs500_album_file_path)
```

You can add the number of times an artist is mentioned on the list

```
my_rs500_count <- lapply(my_rs500_artists$Artist, function(x)(length(which(x == my_rs500_merge_list_2$Artist))))
my_rs500_count <- cbind.data.frame(my_rs500_artists, "No-of-Albums" = unlist(my_rs500_count))

head(my_rs500_count)
          Artist No-of-Albums
1    Marvin Gaye            3
2 The Beach Boys            3
3  Joni Mitchell            4
4  Stevie Wonder            4
5    The Beatles            9
6        Nirvana            3
```

### youtube-dl in R

We have held off on creating the album file path inside each artist file directory.  That's because we will create those folders and the files inside with youtube-dl via R.  R provides a nice little function called ```system()``` that calls the terminal command.

Add the playlist url and id to the full list (now called ```my_rs500_merge_list_2```).  I was successful with the API call, and I am going to assume that the first playlist link returned is the best one.  As noted, you have to insert your own url and id based on how it was acquired.

```
my_yt_id <- lapply(rs500_json_playlist_id , function(x)(ifelse(length(x)==0,NA,first(x)))) # <INSERT LIST OF YOUTUBE IDs FROM STEP ABOVE>

my_yt_url <- lapply(rs500_json_playlist_url , function(x)(ifelse(length(x)==0,NA,first(x)))) # <INSERT LIST OF YOUTUBE IDs FROM STEP ABOVE>

my_rs500_merge_list_4 <-
  data.frame(my_rs500_merge_list_3, "Playlist.ID" = unlist(my_yt_id), "Playlist.URL" = unlist(my_yt_url))

write_csv(my_rs500_merge_list_4, path = "C:/Users/Administrator/Dropbox/rs500/rs500_final_list.csv") #/INSERT/FILE/PATH/HERE/
```

We can then create a function that uses the ```system()``` command in R to download all of the albums.  There is some limit to how quickly each album can be downloaded, so I will set some limits and sleep time into the function.

The command to use in R is:

```
system("youtube-dl --extract-audio --audio-format mp3 -o '/Users/jake_macbook_pro/%(playlist)s/%(playlist_index)s - %(title)s.%(ext)s' <YOUTUBE PLAYLIST ID>")
#system("youtube-dl --extract-audio --audio-format mp3 -o '/INSERT/FILE/PATH/HERE/%(playlist)s/%(playlist_index)s - %(title)s.%(ext)s' <YOUTUBE PLAYLIST ID>")
```

Although the command ran great in the terminal, it had issues running it in R until I included the full path to the directory where it would be saved.  Since we have already solved for this in the ```my_rs500_merge_list_3``` data frame, it will be less of an issue.

```
# - create the system command
my_yt_dl_command <-
  paste0(
    "youtube-dl --extract-audio --audio-format mp3 -o '",
    my_rs500_merge_list_3$File.Path,
    "/%(playlist_index)s - %(title)s.%(ext)s'",
    " ",
    my_rs500_merge_list_3$Playlist.ID
  )

fn.yt.dl.download <-
  function(start = 1, end = 10, command = my_yt_dl_command, sleep = 60){
    for(i in start:end){
      system(command[[i]])
      Sys.sleep(sleep)
    }
  }
```

One other option worth mentioning is that youtube-dl will allow you to access a .txt file with a list of urls or ids.  So save ```my_yt_dl_command``` as a txt file then run the following from the command line:

```
$ youtube-dl --extract-audio --audio-format mp3 -o '/Users/jake_macbook_pro/%(playlist)s/%(playlist_index)s - %(title)s.%(ext)s' -a <LIST.TXT>

# - where LIST.TXT is the filename where the files are saved.
# - according to the documentation for youtube-dl, LIST.TXT is a "File containing URLs to download ('-'for stdin), one URL per line. Lines starting with '#', ';' or ']' are considered as comments and ignored.""
```

## Organize Your Directory, HTML and Review Site

Now that you have downloaded all of your files, take the time to enjoy all of the hard work!  Because you don't always get what you think from youtube-dl, you can examine what files were downloaded by executing the following code.

```
my_artists <- list.dirs(my.dir, recursive = F)
my_albums <- list.dirs(my_artists, recursive = F)
my_songs <- list.files(my_albums, full.names = T)
```

You can also make a basic table with all of the information you found and export as such:

```
kable(my_rs500_merge_list_4, "html") %>%
    kable_material("hover", full_width = F) %>%
    save_kable("rs500.html", self_contained = T)
```

Happy listening!

## Notes & Research

### List of Sites to Automatically Organize Audio Files

* [Organize Music Library: Everything You Need To Know](https://homedjstudio.com/organize-music-library/)
* [Mixed In Key: Software for DJs and Music Producers - Mixed In Key](https://mixedinkey.com/)
* [Platinum Notes - Improve Your Music Collection (Audio Software)](http://www.platinumnotes.com/)
* [5 steps to building and managing a digital music library | ResourceUMC](https://www.resourceumc.org/en/content/5-steps-to-building-and-managing-a-digital-music-library)
* [MetaBliss - the perfect ID3 tag editor for Mac OS X](http://www.metabliss.com/)
* [Mp3tag - the universal Tag Editor (ID3v2, MP4, OGG, FLAC, ...)](https://www.mp3tag.de/en/index.html)
* [beets: the music geek’s media organizer — beets 1.4.9 documentation](https://beets.readthedocs.io/en/stable/index.html)
* [Characters to Avoid in Directories and Filenames | UMC | Michigan Tech](https://www.mtu.edu/umc/services/websites/writing/characters-avoid/)
* [bookdown, My Process | A. Solomon Kurz](https://solomonkurz.netlify.app/post/how-bookdown/)
* [10.3 Deployment | R Markdown: The Definitive Guide](https://bookdown.org/yihui/rmarkdown/blogdown-deploy.html)
* [R Markdown](https://rmarkdown.rstudio.com/)
* [10.5 rmarkdown’s site generator | R Markdown: The Definitive Guide](https://bookdown.org/yihui/rmarkdown/rmarkdown-site.html#a-simple-example)
* [A Step-by-Step Guide: Deploying A Static Site or Single-page App | Netlify](https://www.netlify.com/blog/2016/10/27/a-step-by-step-guide-deploying-a-static-site-or-single-page-app/)
* [Licensing a repository - GitHub Docs](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-repository-on-github/licensing-a-repository?query=move+repository#choosing-the-right-license)
* [gh repo view | GitHub CLI](https://cli.github.com/manual/gh_repo_view)
* [GitCheatSheet_issue#2366_102019-V5](https://training.github.com/downloads/github-git-cheat-sheet.pdf)
* [save_kable: Save kable to files in kableExtra: Construct Complex Table with 'kable' and Pipe Syntax](https://rdrr.io/cran/kableExtra/man/save_kable.html)
* [kableExtra.pdf](https://cran.r-project.org/web/packages/kableExtra/kableExtra.pdf)

### General Information from Researching Article

* [R API Tutorial: Getting Started with APIs in R](https://www.dataquest.io/blog/r-api-tutorial/)
* [Convert JSON URL to R Data Frame - Stack Overflow](https://stackoverflow.com/questions/35120167/convert-json-url-to-r-data-frame) - [DB](https://www.dropbox.com/s/hbrbuz6x1dhoint/Convert%20JSON%20URL%20to%20R%20Data%20Frame%20-%20Stack%20Overflow.pdf?dl=0)
* [Using 'rvest' to extract links - Stack Overflow](https://stackoverflow.com/questions/35247033/using-rvest-to-extract-links) - [DB](https://www.dropbox.com/s/qr2yhggak4uglk7/Using%20%27rvest%27%20to%20extract%20links%20-%20Stack%20Overflow.pdf?dl=0)
* [DuckDuckGo Search Syntax | DuckDuckGo Help Pages](https://help.duckduckgo.com/duckduckgo-help-pages/results/syntax/) - [DB](https://www.dropbox.com/s/c0gcnzrppyag48c/DuckDuckGo%20Search%20Syntax%20%7C%20DuckDuckGo%20Help%20Pages.pdf?dl=0)
* [Grabbing HTML code from URL that needs a delay for webpage to fully load - Stack Overflow](https://stackoverflow.com/questions/37948178/grabbing-html-code-from-url-that-needs-a-delay-for-webpage-to-fully-load) - [DB](https://www.dropbox.com/s/uy93kvz686chu6w/Grabbing%20HTML%20code%20from%20URL%20that%20needs%20a%20delay%20for%20webpage%20to%20fully%20load%20-%20Stack%20Overflow.pdf?dl=0)
* [Not able to scrape a website - General - RStudio Community](https://community.rstudio.com/t/not-able-to-scrape-a-website/17489) - [DB](https://www.dropbox.com/s/75qlv3ydv0wtsum/Not%20able%20to%20scrape%20a%20website%20-%20General%20-%20RStudio%20Community.pdf?dl=0)
* [50+ Advanced YouTube Search Operators (Examples + Tips)](https://seosly.com/youtube-search-operators/) - [DB](https://www.dropbox.com/s/0mn6skdw6raa5ku/50%2B%20Advanced%20YouTube%20Search%20Operators%20%28Examples%20%2B%20Tips%29.pdf?dl=0)
* [Scraping the content of all div tags with a specific class - Stack Overflow](https://stackoverflow.com/questions/48373138/scraping-the-content-of-all-div-tags-with-a-specific-class) - [DB](https://www.dropbox.com/s/rujx4fn8xbp1oxh/Scraping%20the%20content%20of%20all%20div%20tags%20with%20a%20specific%20class%20-%20Stack%20Overflow.pdf?dl=0)
* [html_text: Get element text in rvest: Easily Harvest (Scrape) Web Pages](https://rdrr.io/cran/rvest/man/html_text.html) - [DB](https://www.dropbox.com/s/aiyw6kiamqumjl9/html_text%3A%20Get%20element%20text%20in%20rvest%3A%20Easily%20Harvest%20%28Scrape%29%20Web%20Pages.pdf?dl=0)
* [Web Scraping without getting blocked](https://www.scrapingbee.com/blog/web-scraping-without-getting-blocked/) - [DB](https://www.dropbox.com/s/cmsx10q96ix9g5b/Web%20Scraping%20without%20getting%20blocked.pdf?dl=0)
* [youtube-dl/README.md at master · ytdl-org/youtube-dl](https://github.com/ytdl-org/youtube-dl/blob/master/README.md#readme) - [DB](https://www.dropbox.com/s/wckic3mshvsoqhy/youtube-dl%3AREADME.md%20at%20master%20%C2%B7%20ytdl-org%3Ayoutube-dl.pdf?dl=0)
* [HermanFassett/youtube-scrape: Scrape YouTube searches (API)](https://github.com/HermanFassett/youtube-scrape) - [DB](https://www.dropbox.com/s/ftp43y6v5grr2d5/HermanFassett%3Ayoutube-scrape%3A%20Scrape%20YouTube%20searches%20%28API%29.pdf?dl=0)
* [Most Common HTTP Headers for Web Scraping - Blog | Oxylabs](https://oxylabs.io/blog/5-key-http-headers-for-web-scraping) - [DB](https://www.dropbox.com/s/dli9gy7j73u1ukf/Most%20Common%20HTTP%20Headers%20for%20Web%20Scraping%20-%20Blog%20%7C%20Oxylabs.pdf?dl=0)
* [Configure random proxies with R for scraping - Stack Overflow](https://stackoverflow.com/questions/52172754/configure-random-proxies-with-r-for-scraping) - [DB](https://www.dropbox.com/s/aitlr3zz9uk77x6/Configure%20random%20proxies%20with%20R%20for%20scraping%20-%20Stack%20Overflow.pdf?dl=0)
* [use_proxy: Use a proxy to connect to the internet. in httr: Tools for Working with URLs and HTTP](https://rdrr.io/cran/httr/man/use_proxy.html) - [DB](https://www.dropbox.com/s/opb13xj1bnsv11l/use_proxy%3A%20Use%20a%20proxy%20to%20connect%20to%20the%20internet.%20in%20httr%3A%20Tools%20for%20Working%20with%20URLs%20and%20HTTP.pdf?dl=0)
* [Chapter 37 Intro to rvest | Sports Data Analysis and Visualization](http://mattwaite.github.io/sports/intro-to-rvest.html) - [DB](https://www.dropbox.com/s/4nbsufu6v4lv2c1/Chapter%2037%20Intro%20to%20rvest%20%7C%20Sports%20Data%20Analysis%20and%20Visualization.pdf?dl=0)

### List of Illegal Characters in Filename
```
c("#","%","&","{","}","\","<",">","?","/","$","!","'",""",":","@")

# pound

% percent

& ampersand

{ left curly bracket

} right curly bracket

\ back slash

< left angle bracket

> right angle bracket

* asterisk

? question mark

/ forward slash

  blank spaces

$ dollar sign

! exclamation point

' single quotes

" double quotes

: colon

@ at sign
```
