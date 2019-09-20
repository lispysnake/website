---
title: "Lispy License: A Thingy What Does Stuff (TM)"
date: 2019-09-20T17:26:54+01:00
draft: false
authors: [ikey]
---

# We Made A Thingy

Further to ~~Teh Game Rayzer~~ [The Game Raiser](/the-game-raiser) campaign, we've been working on a license management tool. Long story short, we needed an effective
way to issue unique license keys for our pre-sale licenses. Our constraint at this point is simply linking an email to a unique key, and issuing them by email. As a result,
those lucky early adopters should now have received their license keys to the email address listed via the PayPal transaction. You'll need this key in future when you
want to download and play our upcoming 2D titles (More on that during the next few days..)

{{<figure_screenshot image="/lispy-license-a-thing-what-does-stuff/screenshot_tool" caption="lispylicense tool in action">}}

## lispylicense

We named the project `lispylicense`, which you can find over on [{{<fontawesome fab fa-github>}} GitHub](https://github.com/lispysnake/lispylicense). Promise: The tool is better than
the name. It's implemented in [Golang](https://golang.org/) (Go) and available under the terms of the [Apache-2.0 license](https://www.apache.org/licenses/LICENSE-2.0). Our **eventual**
aim is to have a simple microservice that handles license allocation requests as part of a larger Lispy Snake architecture to streamline player experiences.

The project is currently in a pre-alpha state, thus there are no tagged releases. In the interest of being software engineers that care about such practices, we'll be cutting new
releases in the near future.


## How Does It Work?

If you're not in any way geekishly-inclined, look away now.

A simple database (we're currently just using SQLite3, but others are trivial to support) stores a set of known licenses, which currently just have a description field and a maximum
user count. If the maximum count is less than 0, it is considered an unlimited license (i.e. no stock-limitation). If however, like the `LifetimeLicense.2D` license required for our
Game Raiser, the count if more than 0, we will only allow license assignments up until that point is reached.

Internally the license is simply a [UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) generated using `libuuid`'s `uuid_generate` and `uuid_unparse_lower`. Nothing
too fancy there, then. License keys are linked with an `account_id` (currently, an email address) and an attempt is made to store this in the database, satisfying unique constraints.
This storage is only permitted if the `max_users` threshhold won't be exceeded. If this does in fact succeed, we'll perform the insert, and email the user to let them know what their
license key is.

## Future Plans

Obviously, the current tool is quite simple and shouldn't be relied on by outside parties as we're planning big changes. Amongst them, schema migrations and versioning are included.
The eventual goal is a microservice, which performs license allocation requests when all conditions are satisfied, as well as providing license *verification* APIs. We're considering
time-limited license types with a default duration, which would allow for corner cases such as "30 day passes".

For the mean time, however, we're pivoting back to engine work. Stay tuned, exciting updates to come!

## A Final Word

We just want to extend a massive thank you to everyone who's bought a Lifetime License for 2D Titles from Lispy Snake. Stay awesome. {{<fontawesome fa fa-heart>}}

{{<paypal_2d_game>}}
