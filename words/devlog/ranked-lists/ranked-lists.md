# Ranked Lists in Markdown

Markdown sure is a great way to write content quickly.
I rarely touch word processors these days.
The software is overkill when the content that I write is not going to see a printed page or a PDF.
Visual Studio Code and other text editors can give me a live preview of the output of the Markdown being written.
For editing purposes, I can split each sentence in a paragraph into a new line.
This allows me to better utilize Git to see how my draft has changed over time.
The aspect of being able to track changes to text over time has made me wonder if Markdown is a viable way to create ranked lists.

Jeff Gerstmann of Internet fame has been working on a project where he has been ranking every game released on the Nintendo Entertainment System or NES.
Every Friday, he will play a few NES games and conduct some science.
The science is very methodical and brings up difficult questions.
For example, is "Golf" better than "Dynowarz: Destruction of Spondylus"?
The answer may surprise you.
His list currently contains four-hundred ranked games.
The video archives features this simple numbered list prominently.

Numbered lists also happen to be one of the things that Markdown does an excellent job in rendering.
A list can be created by starting a new line with a number followed by a period, a space and some text.
Lists can start with any number and entries rendered in the list will increment by one.
A list with the numbered entries 1, 7, 64 would be rendered as 1, 2, 3 since the Markdown is being converted to an HTML ordered list.
While this can cause problems if you do actually need to display a numbered list out of order, it is a useful feature to have because the list can easily be changed.
The numbers can even repeat and the resulting list would still keep its order.
Repeating the number one for every entry avoids the issue of having to remember the last number used and makes it as easy as creating an unordered list.
The downside with this approach is that the Git diffs would be more difficult to interpret as Git only tracks changes on the Markdown and not the resulting HTML.

There is a website, [8bitnintendo.science](https://8bitnintendo.science/), that has been tracking Jeff's progress through the hefty Nintendo Entertainment System library.
The site is statically generated using Jekyll.
It features a table containing the ranking and the link to the YouTube video where the science was conducted.
Creating tables in Markdown is annoying, but the author of the site has taken advantage of Jekyll's templating capabilities to automatically create a table from [a CSV file containing the data](https://github.com/vNakamura/8bitnintendo-science/blob/main/_data/list.csv).
Looking through the commit history of that file shows me how the list has changed from week to week.
On [October 4th, 2024](https://github.com/vNakamura/8bitnintendo-science/commit/8dfbac1780e48b341d8ba0343c09be4e9033a148), Jeff was pretty impressed by "Metal Gear".
Reading CSV files is trivial for a computer, but depending on how much a file contains, they can be unwieldy for me to scroll through.
Also, out of the box, Obsidian can not natively display CSV files.
