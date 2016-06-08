---
layout: post
title:  "Object Relationships, Having-Many Through"
date:   2016-06-08 23:57:04 +0000
---


As I near the end of the Object Orientation section of my Learn curriculum, I find myself thinking back and reflecting on the sections that took longest to grasp. OK - actually, I'm stuck on one of the Final Projects and wanted to make *some* kind of progress *somehow*... so let's blog about something! Understanding Object Relationships in code-world was hard for me. In the physical-world, sure... no problem. Actually, it was a concept I remembered being touched on in my Industrial Engineering days in my Information Systems class. In that class, we created objects in Microsoft Access to make our database, utilizing the GUI that automatically created the back-end for us (a.k.a. code-world). If I recall correctly, Microsoft Access let us set-up a database by drawing something like this:

![](http://readme-pics.s3.amazonaws.com/Screen%20Shot%202015-11-03%20at%2012.23.17%20PM.png)

Visually, you can see that an artist has many songs and that each song belongs to an artist and a genre. A given object has many of another type of object (as seen with the triangle at the end of the line from Artist to Song), and that second object belongs to a third object. Therefore, that given first object has many of the third object as well. But what about in code-world???

After reading this lesson, watching videos, it finally seemed easiest once I realized, in code-world, you just work from left-to-right. So, let's start with Artist:

```
class Artist
  attr_accessor :name
 
  def initialize(name)
    @name = name
    @songs = []
  end
 
  def add_song(song)
    @songs << song
    song.artist = self
  end
 
  def songs
    @songs
  end
 
  def genres
    self.songs.collect do |song|
      song.genre
    end
  end
end
```

Working left-to-right, an artist creates a song, which makes sense, since a song wouldn't create itself, nor would it create an artist (or genre for that matter). Simutaneously, The ```#genres``` method iterates over the Artist's ```@songs``` array, and calls the ```#genre``` method on each song in order to collect the genre that is associated to that song.

Now that that's sorted, we get to what tripped me up the most. How do you create the relationship so that an Artist has many Genre's, through Songs?Well, working left-to-right would mean that a Song should add a Genre to itself. In order to do that, we need to code an add_song method into Genre:

```
class Genre
  attr_accessor :name
 
  def initialize(name)
    @name = name
    @songs = []
  end
 
  def add_song(song)
    @songs << song
  end
 
  def artists
    @songs.collect do |song|
      song.artist
    end
  end
end
```

Ah-ha! Now, when a new song is instantiated it can get associated to a genre, and the given genre can add that song to it's collection.

```
class Song
  attr_accessor :name, :artist, :genre
 
  def initialize(name, genre)
    @name = name
    @genre = genre
    genre.add_song(self)
  end
end
```

Now a song knows about the genre it belongs to and a genre knows about the many songs that it has. By associating songs to genres, you don't have to add a given song to an artist's list of songs *and then separately* add a genre to that artist. With a through association, the connection between an artist and its genres will automatically follow. And that's how you do it in code-world!
