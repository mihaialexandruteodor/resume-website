---
title: (Blog post) Creating Supabase project and continuing with the tutorial
date: 2026-02-09
categories: [BLOG]
tags: [blog, Amethyst]     # TAG names should always be lowercase
---
 I have created a free tier **Supabase** project to be used for handling user registration. Supabase offers NoSQL storage and generous free tiers, so it was a better choice than **Firebase**, at least at this point.

<img src="{{ site.baseurl }}/assets/post_images/amethyst/1.png" alt="Supabase project page"/>

<img src="{{ site.baseurl }}/assets/post_images/amethyst/2.png" alt="Supabase book table"/>

### Also, I figured out that I want the app registration to be invite based. This should solve the bot problem and make user moderation way easier, plus it will create a closer community.

- [x] Registration will be done via email whitelisting. A whitelisted email is able to register. 
- [x] Each registered user, after adding a novel entry, gets 3 invite tokens, basically 3 invites, in which they provide 3 email addresses to whitelist (whitelisted addresses will be notified via email).
- [x] Each address whitelisted will have associated the user that whitelisted (invited) them. Whitelisting many users that end up banned will result in a ban of said user's account as well.

I think this system is pretty good, at least in the initial phase.

## Resources used so far:

- [ ] [Getting started with Android and Supabase](https://www.youtube.com/watch?v=_iXUVJ6HTHU)
- [ ] [https://supabase.com/docs/reference/kotlin/introduction](https://supabase.com/docs/reference/kotlin/introduction)
- [ ] [Supabase](https://supabase.com)