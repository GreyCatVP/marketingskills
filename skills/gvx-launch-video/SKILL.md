---
name: gvx-launch-video
description: "When a project, client site, or product ships and the user wants a short launch video with share copy. Use when the user says 'launch video,' 'brag video,' 'announce this launch,' 'make a promo for [client],' 'share copy,' or 'we just shipped.' Wraps the /brag-slim skill with GVX conventions: output folder, file naming, tone by audience, and what must be checked before delivery."
metadata:
  version: 1.0.0
---

# GVX Launch Video

Turns a shipped site or product into a 20-second video with music plus share copy, using the `brag-slim` skill (install it separately: `npx skills add https://github.com/garyvanhorn/brag --skill brag-slim -g`).

## Conventions

- **Tone by audience.** GVX's own work uses the punchy default. Regulated or conservative clients (banks, law, healthcare) use a polished, steady tone with no hype.
- **Reuse the real thing.** Use the live site's own fonts, colors, copy, and wordmark. Invent nothing except a cursor.
- **Output folder:** `~/Code/Brags/<Client or Project Name>/` containing `brag.mp4`, `brag.jpg` (poster), `share-copy.txt`, and `brag-plan.md`.
- **Length:** 15-25 seconds, 1920x1080, 30 fps, five scenes, last scene ends on the call to action.

## Workflow

1. Read the client's `.agents/product-marketing.md` and `.agents/gvx-client.md` if present.
2. Run `/brag-slim` on the live URL with the tone above.
3. Check the stills before rendering: nothing clipped, text readable for its time on screen, no half-faded frame used as the poster.
4. Save the four files to the Brags folder.
5. Show the client approver the video **before** it is posted anywhere. Share copy uses only facts from their site.

## Related Skills

- **social**, **video**: posting and format variants
- **gvx-client-onboarding**: step 6 of the first-two-weeks plan
