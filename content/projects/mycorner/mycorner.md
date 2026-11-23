---
title: MyCorner
status: In Progress
summary: My personal anime streaming corner, built from playlists that official distributors already post on YouTube.
tech: [HTML, CSS, Vanilla JS, YouTube Data API v3, GitHub Pages]
link: https://raagassteven6-max.github.io/MyCorner/
repo: 
cover: 
date: 2026-11-11
---

MyCorner is my own little anime streaming site. I wanted one place to watch the shows I like without hopping between channels, so I built it.

It's plain HTML, CSS and JavaScript. No framework, no build step. I'll be honest: most of the frontend was vibe coded through a command-line AI, so my real work is the backend side. How the data gets in, where it's kept, and what happens when something breaks.

## Where the shows come from

Everything comes from legitimate distributors that publish full series on YouTube on purpose: Muse, Ani-One, Made by Bilibili and the Gundam Channel International. I don't host a single video. MyCorner only reads their public playlists through the YouTube Data API and lays them out the way I like.

## The API key and the catalog

Fetching playlists needs an API key, and I'm not putting mine on a public page for anyone to burn through. So I type my own key in when I want to refresh things.

Whenever I fetch, I export the latest catalog and import it back later. That way anyone who lands on the site without a key still sees real content, the last catalog I saved. They just can't refresh it themselves.

## Why there's no backend server

I looked at cloud workers and decided against them. I only pull new content once in a while, so a server running all day for that felt like paying rent on an empty room. A static site on GitHub Pages and a catalog file does the job.

## What I'm still figuring out

- Keeping the catalog fresh without me remembering to refresh it
- Handling playlists a channel removes or makes private
- Making the browse and search side feel better on a phone

> I built it for myself first. If you stumbled in here, you're welcome to look around.
