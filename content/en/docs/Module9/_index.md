---
title: "Module Nine: Hands-on With Text Analysis"
linkTitle: "9: Hands-on With Text Analysis"
weight: 9
description: >
---
## 18 March 2021

{{< figure src="network.jpg" height="5" class="text-center">}}

{{% pageinfo %}}
**Important Proviso:** This week you had your "two days" of study break due to COVID-19. I genuinely want to use those two days in the spirit that they were intended, so we won't have readings in class. What we will have is a short video introducing you to Python, a way to boot up your own notebook, and then we'll do some quick exercises and discussion together in Teams. Catch your breath and you can also get caught up on this or other courses.
{{% /pageinfo %}}

## Readings for this Week

**NO READINGS THIS WEEK BUT WE WILL HAVE OUR MEETING AS SCHEDULED.** 

Please just watch the video when it is posted here (I will do so before the week opens).

## Want to Meet with Me?

As always, you can book a 30-minute meeting with me via Calendly. Use this link [here](https://calendly.com/i2millig/30min). If there are no times that are available, just send me an [email](mailto:i2millig@uwaterloo.ca) and we can work something out. 

This will create a Microsoft Teams appointment. The URL for the Teams link will be in the calendar invitation e-mailed to you.

## Hands-on Lesson

### Getting Text and Playing with It

1. Find a "Collection" at the [Internet Archive](https://archive.org) that you will use. Make sure to keep the text string from the URL. For example, I will use [The Economist](https://archive.org/details/pub_economist).

2. Let's start by downloading the plain text of a file. See below.

{{< figure src="fulltext.png" height="5" class="text-center">}}

3. We'll then play with the easy way to explore: by putting it into [Voyant Tools](https://voyant-tools.org). You can either upload the text file or paste it into the "reveal text" box.

{{< figure src="voyant.png" height="5" class="text-center">}}

4. We will then work with Voyant together.

### Let's Scale this up Computationally

1. Start up a [Google Collab](https://colab.research.google.com) notebook. Click on "New Notebook." It should look like this.

{{< figure src="empty.png" height="5" class="text-center">}}

2. Let's make sure it works. You can add "Text" and "Code" panels. We will add a "Text" panel explaining that this is HIST 640. Then we will add a "Code" panel with the following piece of code.

```python
print('hello world')
```

When you hit the "play" button or you press Ctrl+Enter, it will run that cell.

{{< figure src="hello-world.png" height="5" class="text-center">}}

Huzzah. It is working!

3. Now we want to work with **Internet Archive** data. Luckily, the Internet Archive has a Python library! A library is a set of commands that we can use to work directly with the Internet Archive.

Let's install it by running the following cell.

```bash
!sudo pip install internetarchive > /dev/null
```

4. Now let's work with your collection. I am going to use the *Economist* in these examples, _but_ you should swap in your own collection. We'll have fun.

First up, let's see what resources we have in a collection. This also makes sure that the library is working.

```python
!ia search "collection:pub_economist" --itemlist | head
```

You should see something like:

{{< figure src="results.png" height="5" class="text-center">}}

Now let's look at an individual **item**. It can get a bit complicated, but here you see is the actual item itself. Let's see what formats we have available.

```python
!ia metadata --formats sim_economist_1843-09-02_1_1 
```

{{< figure src="formats.png" height="5" class="text-center">}}

I'll walk you through what these are. 

5. Let's download the text.

```python
!ia download sim_economist_1843-09-02_1_1 --format='DjVuTXT'
```

You will see `success` when it is done downloading!

{{< figure src="success1.png" height="5" class="text-center">}}

We can find it in our "downloads" folder on the side of Google Collab.

{{< figure src="files.png" height="5" class="text-center">}}

Let's download the text. See, we could also pop this into Voyant if we wanted to. Or we could also download lots of text to pop into Voyant.. the possibilities are limitless.

6. Let's say we want to build an epic PDF collection. Let's try the same command as above, but this time we'll change the format to PDF. It would look a bit like this:

```python
!ia download sim_economist_1843-09-02_1_1 --format='Text PDF'
```

Let's imagine we wanted to download _all_ of them. 

```python
!ia download --search 'collection:pub_economist' --format='DjVuTXT'
```

7. Now we'll play a bit around with this.

### How can we analyze text in Python?

We'll start with 'strings' - little phrases, just to conceptually see what's going on. We'll start with some basic Waterloo phrases, and then we will eventually load in our _Economist_ downloaded text file.

I am adopting this from this great [Normalizing Textual Data with Python](https://programminghistorian.org/en/lessons/normalizing-data) lesson from the _Programming Historian_, written by William J. Turke land Adam Crymble. Shout out to their CC-BY 4.0 license for letting this happen.

1. Let's create some strings:

```python
wordstring = 'The University of Waterloo has 7,000 geese on its campus. '
wordstring += 'While geese are normally friendly, they should be approached with caution. '
wordstring += 'However, I really like Geese.'
```

Now if we type

```python
wordstring
```

We will see:

{{< figure src="geese.png" height="5" class="text-center">}}

2. We can also make it into a list:

```python
wordlist = wordstring.split()
```

and then type

```python
wordlist
```

3. Let's count the words!

```python
wordfreq = []
for w in wordlist:
    wordfreq.append(wordlist.count(w))
```

And then let's see what the string, the list, the frequencies, and the pairs are.

```python
print("String\n" + wordstring +"\n")
print("List\n" + str(wordlist) + "\n")
print("Frequencies\n" + str(wordfreq) + "\n")
print("Pairs\n" + str(list(zip(wordlist, wordfreq))))
```

4. Uh oh! Here's a problem. There are 3 geese in the string, but only two show up. Now we've got to begin **NORMALIZING** it.

We can make a string lowercase by doing this:

```python
wordstring.lower()
```

Let's try to re-do all the above, but make the string lowercase. It might take a few minutes so take your time.

We then run into the problem of periods. Check this out, we can do a simple find and replace as well.

```python
wordstring.replace(".","")
```

That would remove all the periods and replace them with nothing.

### Let's Bring it All Together

1. Let's bring that text file into Python. To do so, we can click on the three dots next to the file and select "copy path."

{{< figure src="path.png" height="5" class="text-center">}}

Let's put that into a new variable called "filename"

```python
filename = "/content/sim_economist_1843-09-02_1_1/sim_economist_1843-09-02_1_1_djvu.txt"
```

Now let's open it, using this:

```python
from pathlib import Path
txt = Path(filename).read_text()
```

And let's see what's in there, using the `print` command:

```python
print(txt)
```

Now that's like our Goose string from before! Let's play around a bit with counting some words in it, etc. Notice here I put the `lower()` command in with the `split()` command to make it a bit more streamlined.

```python
wordlist = txt.lower().split()

wordfreq = []
for w in wordlist:
    wordfreq.append(wordlist.count(w))
    
print("Frequencies\n" + str(wordfreq) + "\n")
print("Pairs\n" + str(list(zip(wordlist, wordfreq))))
```

We'll just play a little bit and discuss here. 