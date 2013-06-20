# Easychord

We'll describe a system for representing simple chords and songs on a guitar, and hopefully grow it into a way of optimizing tunings and hand positions. We'll start with a basic "rhythm guitar" song. Normally these are written as a progression of chords to be played by the guitarist, arranged with notation explaining how many measures each chord should be played. Chords are in turn composed of individual notes. Outside of the selection of chords, the guitarist is free to choose how to play each chord to play, including and up to choosing the chord form and strumming pattern. We hope to help beginner and intermediate players pick chord forms that are easy to learn and play.


# Definitions: Notes, Pitches, Chords, Songs, Strings, Tunings, Chord Forms

Here we assume the notes all belong to the 12 note equal temperament tuning system. This is a decent assumption for the western 6 string guitar, as most have frets placed according to this pattern.

## Notes

We start out with a set of 12 notes, fundamental to the 12 note equal temperment system:

C, C#, D, D#, E, F, F#, G, G#, A, A#, B. 

We call these 'note names'. By convention we start out with C, but this choice is arbitrary. For convenience, we take the 12 element set {0.. 11} and call it 'Note'. We call the elements of Note 'note numbers', and use them to reperesent the note names. This will allow us to use modular arithmetic to do useful things like calculate musical intervals. 

Each note is not a particular pitch (aka tone or frequency), but instead represents a "pitch class", a set of all octaves of a particular note. [clarify this?]

We will not concern ourselves with how notes are turned into physical pitches and frequencies, only with how they are represented on the neck of a guitar.


## Pitches
 
We represent each pitch as a pair (n, o) :: (Note, Nat), which consists of note number n and an octave number o, which is just an element of the natural numbers {1 2 ..}. Let 'Pitch' denote the set of pitches. We may also represent pitches as a note name concatenated with an octave number. For example, E2 string on a standard tuned guitar is written as (4, 2).

## Chords

We let each chord c :: Set(Note, 6) be an arbitrary set of at most 6 notes. A given chord can be 'voiced' in more than one way by using pitches from different octaves. We use the name 'Chord' to denote the set of all chords. Note, this not only includes the usual melodic chords, but any arbitrary combination of at most 6 notes. In classical music theory, chords may contain more than 6 notes, but here we impose an arbitrary limit for the 6 string guitar.

Example: The 'D Major Seventh' chord {2 6 9 1}, aka {D F# A C#}, may be voiced in many ways, for example using the pitches {(2,2) (6,2) (9,2) (1,2)}, or {(3,3), (9,3), (1,4)}. We will see that while the second voicing is playable on a standard tuned guitar, that is not true of the first voicing.

## Basic Rhythm Guitar Songs

Lets start introducing more guitar specific elements, namely the Rhythm Guitar song. A basic rhythm guitar song S consists of a sequence [c_1 .. c_n] of chords. To simplify things, we say that such songs are in in 4/4 time, and say that each chord is played for a beat. These are very basic songs, like you might play in a campfire singalong. The set of all songs is denoted Song :: [Chord], ie, it is the set of all sequences of chords.

Given a song, a player must choose a tuning. We address this next.

## Strings

A tuned string with frets is denoted s = (b, n), and consists of a base pitch b, and a number n of frets available to the string. We say String = (Note, N) to define the set of all string/fret configurations. The player may use the string and frets together to play n+1 pitches. A tuned string's underlying note is called it's "base note".

Example: a string s = ((8,2), 10) starts at G#2, and has pitches [G#2 G2 .. B2 C3 .. E3]. The base note of s is just 8.

## Tunings

Let Tuning = String^6. That is to say, a guitar tuning consists of an ordered tuple of 6 tuned strings. A simplified tuning consists of 6 base notes and no other string information: SimplTuning = Note^6. We have that Tuning is isomorphic to (SimplTuning, N^12).

Example. The standard turning on a guitar is (E2 A2 D3 G3 B3 E4), where each string has 20 frets. We'll use our numerical notation and define the standard tuning as

 S = [((4 2) 20) ((9 2) 20) ((2 3) 20) ((7 3) 20) ((11 3) 20) ((3 4) 20)). 

The simplified standard tuning is thus [4, 9, 1, 7, 11, 4].

So far we have notes, pitches, chords, simple songs, tuned strings, and guitar tunings. Now comes a harder part: to play a chord, the player must realize it using a chord form.


## Chord Forms
 Given a chord c and a guitar tuning G, let F(c, G) be the set of ways we can express the Notes of c using G's strings. We call these chord forms. They are chord voicings that are realizeable on neck of a guitar, along with a particular way to realize them. We say F(c) = F(c, S) is the set of standard chord forms of c. 

Now, each chord form f \in F(c, G) may be represented as a 6-tuple of finger positions required to play that form. Two caveats: if a given string is played open in the chord form, we assign a 0, and if the string is not used at all, assign a placeholder symbol x. We call this representation the chord form notation. The example should make this clearer:

Example: Take the D Major chord, Dmaj = {9, 2, 6}. We may play this chord using the chord form f_1 = (x, x, x, 2, 3, 2) and . Another realization is f_2 = (x, x, 7, 7, 7, x). These two chord forms in fact play the exact same pitches.

If two chord forms f_1 and f_2 represent not only the same chord, but also sound the same pitches, we say f_1 and f_2 are equivalent. It's clear that equivalence of chord forms is an equivalence relation on F(c, G), and thus we may partition F(c, G) into classes of equivalent forms. 

Let f be a chord form. There is a chord c such that  F(c, G) contains f. Thus, given any form f, we write [f]_G to denote its class of equivalent forms in tuning G, and we write c(f, G) to denote its underlying chord. We write [f] to denote the equivalence class of f under standard tuning, and c(f) to denote its underlying chord. 

# Ordering Chord Forms

Finally, write f^*_G to denote the set of "efficient" or "easy" chord forms equivalent to a given chord from f. This assumes, for instance, that it is possible to place a bounded preorder on [f]_G. That is, we may suppose there is a relation ≤ on [f]_G such that for two chord forms f_1, f_2 \in [f]_G, we write f_1 ≤ f_2 if it makes sense to say f_2 is 'easier', or 'more efficient' to play than f_1. In this case f^*_G consist of the maximal elements of the preoprder ([f]_G, ≤).  This preorder may not be well defined in general, since it is defined by the player's abilities and preferences, and subjective an changes from playing session to playing session as the player learns. To define it, we would likely depend on assumptions or data about how and individual player would rate different hand positions. We'd likely strengthen the assumptions to allow a full utiliyu function and attempt to estimate one such function that is applicable to a given player, or a class of players of a given skill. If we use a rational objective function, f^*_G becomes a full pre-order, even.

Example. Between the Dmaj chord forms f_1 and f_2 above, at the initial time of writing this (a couple of days ago), I found f_1 easier than f_2, since I was not very adept at 'barring chords'. Thus at the time, f_2 ≤ f_1 was the case. Taking this as a challenge, I am now about even in skill in both forms of the chord, and hence f_1 ≤ f_2 also applies.

## Form height

One simple metric of chord form complexity is the vertical height metric - ie, simply count the max spread between finger positions as the metric.

Example: f_1 has a height of 1, while f_2 has a height of zero.

References:

https://en.wikipedia.org/wiki/Pitch_class

https://en.wikipedia.org/wiki/Equal_temperament#Twelve-tone_equal_temperament

