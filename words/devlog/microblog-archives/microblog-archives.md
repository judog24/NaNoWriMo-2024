# Microblog Archives

The two year anniversary of the launch of my Mastodon instance, [cheddarcrackers.club](https://cheddarcrackers.club/), is approaching.
I have been decently active on there, making around three thousand posts on my primary account.
I had been using another microblogging service for over a decade before I made the switch.
I was slightly disappointed that I was leaving such a big digital footprint behind, but I at least had access to a downloadable archive that I was able to search through.

The archive is mostly contained in a 22 MB javascript file.
Nearly 800,000 lines of code comprise 16,000 posts.
There is a lot of metadata that accompanies each post.
The archive even contains links to the original posts but as the site continues to change, who knows how long they will be viewable.
Posts that are set for public viewing now require an account to access.

The offline post archive has the same struggles as viewing the live profile.
Scrolling through a seemingly endless amount of posts is daunting.
I can narrow posts down to specific date ranges.
I can also search for posts if I can remember what I wrote in them.
The social media landscape changed over the years that I was on this classic microblogging site which really impacted the long term readability of these posts.

Early posts in my archive used to rely heavily on hashtags.
Eventually, the site got good enough to determine topics by analyzing the text.
This was convenient when posting about big events such as E3 and the Super Bowl.
Posts about Nintendo Direct announcements get lost in the mix as they can happen on random days throughout the year.
I also have quite a few posts that are just images.
No text or even alt text to give context.
Today, I provide alt text for all of my Mastodon posts.

Seeing as how I have complete access to my post archive, I have the ability to display the posts however I want.
My goal is to create a statically generated site that can sort my posts by year and month.
I also want to the ability to assign "events" to posts to create specific pages that group posts about a topic together.

My plan is to use Hugo to generate the site which uses Markdown files for content.
Post metadata can be converted from JSON to YAML front matter.
The content of each microblog post can be contained in the body of the Markdown file.
The metadata also contains a unique id for posts which can be used to create unique Markdown file names.
Datetime strings might be a more human-readable approach.
It looks like I might have to do some transformations to the post timestamps to get them to play friendly with Hugo's front matter requirements.

## Parsing Timestamps

Hugo has [a list of supported date formats](https://gohugo.io/content-management/front-matter/#dates) which mostly follow the ISO 8601 format.
This is great.
My post archive for some strange reason does not adhere to this standard.
Oddly enough, one of the pieces of metadata is a `editableUntil` field which follows the standard perfectly.

```json
"editableUntil" : "2022-11-06T03:48:14.000Z"
```

The all important `created_at` field uses another format.

```json
"created_at" : "Sun Nov 06 03:18:14 +0000 2022"
```

I find it interesting that they decided to make this UTC timestamp more human readable.
Also interesting is how the metadata field goes from camelCase to using underscores.
All of the dates follow the same format.

I will be using PowerShell to ingest the data and create the Markdown files.
There are probably fancier ways of converting this string to a format that is convenient to use, but I am just going to write a simple function to manipulate the string.

```PowerShell
function Import-PostDate {
    param (
        $dateString
    )

    $parts = $dateString.split(' ');

    $month = $parts[1]
    $day = $parts[2]
    $year = $parts[5]
    $time = $parts[3]

    $cleanDate = "$month $day $year $time"
    $cleanDate
}

$test = "Sun Nov 06 03:18:14 +0000 2022"
$postDate = Get-Date -date (Import-PostDate -dateString $test)
$postDate.ToLocalTime()
```

The resulting output after converting the date to local time is `Saturday, November 5, 2022 8:18:14 PM` which is the date the original post displays for me.
Hugo should be able to convert to any time zone with the proper formatting which leads to the next step of the PowerShell code. 
Since the string is now able to be interpreted by PowerShell's Get-Date cmdlet, I can take advantage of it to get the exact formatting that I need for Hugo.

```PowerShell
$postDate = Get-Date -date (Import-PostDate -dateString $test) -Format "yyyy-MM-ddTHH:mm:ssZ"
```

This will now display `2022-11-06T03:18:14Z` as the output.
I will probably need to fiddle around the dates again to sort the posts into relevant folders but for now I can begin the fun part of having PowerShell read my post archive.
