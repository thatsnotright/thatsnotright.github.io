---
layout: page
wide: true
title: TwerkIt
description: An Android workout app that pairs with an Amazfit Helio Ring for live heart rate, plans the week from YouTube workout videos and dumbbell routines, and tracks effort by muscle group.
image: /assets/img/twerkit-card.jpg
---
TwerkIt is the workout app I wanted and couldn't find. It reads live heart rate straight off an
Amazfit Helio Ring, builds the next few days from whole YouTube workout videos and four dumbbell
routines, and keeps a running tally of how hard each muscle group has been worked so the plan
knows what to give a rest.
{: .lede}

- **Runs on** Android, built with Flutter
- **Pairs with** Amazfit Helio Ring, over Bluetooth LE
- **Stores** everything on the phone, in SQLite
{: .facts}

(the name is silly; the squats are not.)
{: .pen}

<section class="feature">
<div class="feature-text" markdown="1">

## Pairing a ring that only talks to its own app

The Helio Ring is meant to talk to Zepp and nothing else. TwerkIt talks to it directly. I worked
out how from a Bluetooth capture of the official app talking to my own ring; no third-party code
went into it.

- **Handshake.** An elliptic-curve key exchange on `sect163r2`, then an AES-128 proof built from
  the ring's random challenge and its 16-byte per-device key, which you copy from your Zepp
  account once.
- **The key stays put.** It lives in the Android keystore. It's never written to the database or
  the logs, and it never goes over Bluetooth.
- **Rings Zepp already holds.** A ring connected to Zepp stops advertising, so a scan alone won't
  find it. TwerkIt also lists rings the phone is already bonded with.
- **Zones.** Set your resting and max heart rate and the five zones are worked out from them.

</div>
<figure class="phone">
  <img src="{{ '/assets/img/twerkit/ring.webp' | relative_url }}" width="600" height="1332" loading="lazy"
       alt="Ring and heart rate screen: a paired Amazfit Helio Ring found by a scan, a saved auth key, and resting and max heart-rate fields with the computed zone thresholds.">
  <figcaption class="pen">scan, pair, paste the key once</figcaption>
</figure>
</section>

<section class="feature">
<div class="feature-text" markdown="1">

## A video workout, with your heart rate under it

Start a planned video and it plays inside TwerkIt, with the ring's heart rate underneath: the
current number and zone, a five-minute trace over coloured zone bands, how far through the video
you are, your average, and time spent in zone 4 and above. If you'd rather use the YouTube app,
it opens in split screen beside TwerkIt and the clock keeps running.

When the workout ends, the readings become a **training load**: each minute counts once in
zone 1, five times in zone 5, and not at all below zone 1. If the ring drops out, the gap stays
a gap. TwerkIt never fills it in with a guess. The planner works from these numbers, so a brutal
Wednesday makes for a lighter Thursday.

</div>
<figure class="phone">
  <img src="{{ '/assets/img/twerkit/video-workout.webp' | relative_url }}" width="600" height="1184" loading="lazy"
       alt="Video workout screen: the YouTube video playing at its 'Workout begins' title card, with 85 bpm below zone 1, a heart-rate trace over zone bands, and 3:22 of 39:19 elapsed.">
  <figcaption class="pen">warm-up done, here we go</figcaption>
</figure>
</section>

<section class="feature">
<div class="feature-text" markdown="1">

## A week planned from YouTube

Point TwerkIt at a YouTube channel and it builds a catalog of whole workout videos. Anything under
five minutes is left out: shorts, trailers, the occasional recipe.

- **Chapters.** It reads chapters from each video's description using YouTube's own rules, then
  reads the movement names listed under headings like "Block 1". Each movement is matched to
  muscles from an exercise catalogue first, with a keyword list as backup. When neither
  recognises something, TwerkIt asks you instead of guessing.
- **The week has a fixed shape.** Videos on Monday, Wednesday and Thursday, upper body on Tuesday,
  lower body on Friday, rest at the weekend. What changes is which video fills each slot.
- **Picking a video.** It favours the muscles that have rested longest and won't repeat a video
  within two weeks. A video that mostly hammers what tomorrow's lifting needs gets dropped, or
  ranked down if the overlap is smaller.
- **Every pick says why**, for example "Back and biceps rested longest; never done". Videos play
  inside TwerkIt, or in the YouTube app in split screen next to it.

</div>
<figure class="phone">
  <img src="{{ '/assets/img/twerkit/plan.webp' | relative_url }}" width="600" height="1332" loading="lazy"
       alt="Plan screen: today is the Lower B strength routine with its five exercises, then two rest days, then a 40-minute back and biceps YouTube video picked because back and biceps rested longest.">
  <figcaption class="pen">the next four days, with reasons</figcaption>
</figure>
</section>

<section class="feature">
<div class="feature-text" markdown="1">

## Strength days, set by set

The lifting half of the plan is four dumbbell routines: Upper A and B, Lower A and B. Upper
alternates every week. Lower runs A, B, B, A over four weeks, so the Tuesday and Friday pairing
doesn't repeat on a two-week cycle. Every fourth week is a deload at 70% of working weight.

Log each set's weight and reps as you go. The previous column shows what you did last time, and
the ring's heart rate sits at the top. Every movement knows which muscles it works and roughly
how hard it is, so a logged set counts toward each muscle's history the same way a video chapter
does.

</div>
<figure class="phone">
  <img src="{{ '/assets/img/twerkit/strength.webp' | relative_url }}" width="600" height="1332" loading="lazy"
       alt="Lower B strength session at 157 bpm: two sets of goblet squats logged at 35 pounds, a third set ready, and the remaining exercises collapsed below.">
  <figcaption class="pen">two down, ten to go</figcaption>
</figure>
</section>

<section class="feature">
<div class="feature-text" markdown="1">

## Effort by muscle group

Everything ends up in one per-muscle history: video chapters, logged sets, and sessions with or
without the ring. The muscle map shades a front and back body by how hard each group has worked
over the last two weeks, compared with whichever group worked hardest. You can switch it between
heart-rate load and plain minutes.

This is the same history the planner uses. When it says your glutes and quads have rested
longest, this is where it got that from.

</div>
<figure class="phone">
  <img src="{{ '/assets/img/twerkit/muscles.webp' | relative_url }}" width="600" height="1332" loading="lazy"
       alt="Muscles worked over the last 14 days: front and back body maps shaded by load, with core, quads and glutes hottest, followed by shoulders, triceps, biceps, chest, back, hamstrings and calves.">
  <figcaption class="pen">core day, every day, apparently</figcaption>
</figure>
</section>

<section class="feature feature--pair">
<div class="feature-text" markdown="1">

## Track anything else

Every workout, video or strength, adds a row to the **Activity tracker** on its own: what it was,
how long it took, and a link back to the video, grouped by day. Anything off-plan, like a run or a
hike, takes one tap to log from Today. Each exercise keeps its own tracker of sets, too.

Beyond that, trackers are yours to make:

- **Typed columns:** numbers with a unit, text, a choice from your own list, or a duration.
- **A daily target** if you want one, shown as progress on the list (0/3 coffees, 2/2 readings).
- **A spreadsheet grid** with a quick-entry row at the top, plus CSV import and export.
- **Templates** to start from. Blood pressure is just a tracker built from one.

</div>
<figure class="phone">
  <span class="phone-pair">
    <img src="{{ '/assets/img/twerkit/activity.webp' | relative_url }}" width="600" height="1332" loading="lazy"
         alt="Activity tracker grid listing recent workouts by day: YouTube videos and strength routines with their durations and video links.">
    <img src="{{ '/assets/img/twerkit/new-tracker.webp' | relative_url }}" width="600" height="1332" loading="lazy"
         alt="New tracker form building a Coffee tracker with a daily target of 3, a Cups number column, and a Kind choice column offering drip, espresso and cold brew.">
  </span>
  <figcaption class="pen">workouts log themselves. coffee counts too.</figcaption>
</figure>
</section>

## Also in there

Blood-pressure readings imported from CSV or typed in by hand, with a chart, and an activity
calendar of every day's training.

TwerkIt is built for one phone and one ring, and it isn't on the Play Store. The video workout was
captured on my phone with the real ring. The other screenshots come from a desktop build running
real workout history, with a simulated ring standing in for the real one.
