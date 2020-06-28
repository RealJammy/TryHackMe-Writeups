# This is my writeup for the OSINT room in TryHack Me!

## Part 1: Finding the avatar.

At first, we're presented with one simple image.

https://github.com/RealJammy/TryHackMe-Writeups/blob/master/OhSINT/WindowsXP%20(1).jpg

The first thing that you can do when presented with an image, is to check for any EXIF data, notes sometimes left by photographers.

There are many ways to do this, the most common of which being exiftool, however, you can view metadata in GIMP, so I decided to use that.

When we do this, we get some co ordinates, and a username, as seen here:

```
54, 17.68778N
2, 15.022104W
OWoodflint
```
Of these 3, this username took my interest the most.

This username allowed me to find his Twitter,Github and Wordpress.

https://oliverwoodflint.wordpress.com/author/owoodflint/

https://twitter.com/owoodflint?lang=en

https://github.com/OWoodfl1nt

The Twitter took me by interest the most, and then I saw that he had a cat profile picture! It was accepted as the answer.

#### Flag 1: Cat


<hr>

## Part 2: Where is OWoodflint? 

From here, there are 2 paths that you can go down. One allows an easier start to question 3, but is slightly harder, and the second path is much easier, but means you have to go back. I will show both paths.


### Path 1: SSID Stalking

With this discovery of the twitter, maybe he might have mentioned a tweet that allows us to grab his location.

```
From my house I can get free wifi ;D

Bssid: B4:5D:50:AA:86:41 - Go nuts!
```

This tweet was the nugget that we needed to go grab his city.

Using wigle.net, a quick search shows us that he is located in London.


### Path 2 : Github Gains

Remember in Part One how we found a GitHub? This becomes very useful here.

In Woodfl1nt's repository, there is a README.md that states the following:

```
Hi all, I am from London, I like taking photos and open source projects.

Follow me on twitter: @OWoodflint

This project is a new social network for taking photos in your home town.

Project starting soon! Email me if you want to help out: OWoodflint@gmail.com


```


This gives us two things, an email (which we're gonna need later), and a city,  London, which is the flag!

#### Flag: London

<hr>

## Part 3: Scrolling and zooming

This part was incredibly simple. Remember in Part 1, how we searched the BSSID? Now, we just have to scroll, and find the SSID ,using the same tool.

#### Flag: UnileverWiFi


<hr>

## Part 4: Email Address!

If you look back to the second path of Part 2, you can see how we get to the email. :p

#### Flag: OWoodflint@gmail.com

<hr>

## Part 5: Is this really a challenge?

As I mentioned in Path 2, Part 2, we got the email on GitHub, making the flag:

#### Flag: GitHub


<hr>

## Part 6: Happy Holidays

In Part 1, we found 3 puzzle pieces. We have already used 2 of them, now to use the final one!

If we go to the wordpress blog that we found in part 1 (https://oliverwoodflint.wordpress.com/author/owoodflint/), we see that he is in New York. This obviously isn't where he lives, so must be where he went on holiday! 

#### Flag: New York


<hr>

## Part 7: The Password Pickle

This final challenge stumped me, at first I had no clue what to do, so I thought back to basics, viewing the source code.

``` html
<p>Im in New York right now, so I will update this site right away with new photos!</p>



<p style="color:#ffffff;" class="has-text-color">pennYDr0pper.!</p>
```

This final part showed us a piece of text that was hidden in white. Using the maturity that I have gained over the past 14 years of life, I decided to change `#ffffff` to a visible colour like `#696969` (mature, I know).

This displayed the text: pennYDr0pper.!, which could be his password. I crossed my fingers and hoped, is this it? And it was!

#### Flag: pennYDr0pper.!

<hr>


## Final Remarks

What did this room teach me? That everything can be found online, from one simple oversight. Think about this, we started this room with the Windows XP background, and think about how much we could get off this one piece of information.

We got his:

- Full Name
- Twitter
- Email
- Github
- City
- Holiday Destination
- Personal Blog
- SSID 
- Password


If an attacker knew their SSID, they could possibly rob his house, knowing that he isn't home. Futhrermore, this email and password being left out may have allowed an attacker to log into his gmail, and delete it, virtually nuking his online presence. If Woodflint used the same password everywhere, an attacker could fire off an angry tweet at his employer, possibly getting him fired from his job. And all this came from one, innocent image


Now, what should you take away from this?

- Never leave your password out for anyone to see
- Use common sense when talking about holidays on social media publically
- Don't give out network details online
- Always strip your EXIF data

And the most important of all:

- Just be smart on the internet. Imagine that everything you say is being broadcast on a megaphone that everyone can hear. That's exactly what happens with the internet.
