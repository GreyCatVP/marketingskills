---
name: caveman-marketing
description: When the user asks to simplify an existing ad, landing page, signup flow or screen, or says "caveman this page", cut each screen to one picture, at most 15 words and one action. Keep the existing claims. For new copy from a brief, see copywriting. For broader page diagnosis, see cro.
license: MIT
metadata:
  author: 5dive team
  version: 1.0.0
---

# Caveman Marketing

Simplify an existing screen while keeping its factual claims. Input can be a URL, screenshot or pasted copy. Use the rules on selling screens and signup steps. Keep the information a user came to read in search results, inboxes and dashboards.

## Inspect the input

1. Read the supplied page or each step of the flow.
2. State what it is, what the visitor gets and where they act. Mark any missing answer.
3. Count words, filled buttons, other exits and steps before the visitor gets the result.
4. Name the one job and its audience. If the offer cannot be stated, explain the gap before rewriting.

## Rewrite the screen

- Put a picture in the top half on a phone, or the left half on desktop. Show the result happening. Label the picture only where needed.
- Keep at most 15 visible words, with at most 6 in a single-sentence headline. Count buttons, field placeholders, small links, step bars and words inside the picture. Exclude emoji, logos and fixed platform text.
- Use one filled button with 👇 above it. Its label says what happens next. Keep other links as small grey text. Nothing in the picture should look like another button.
- If typing is the action, keep one field directly above its button. The field placeholder counts toward the word limit.
- If choosing is the action, show at most 6 common options as tiles, with the rest behind a small More link. Tapping a tile advances the flow, so omit the filled button.
- Show a step bar in a signup. Ask one thing per screen and keep no more than 3 steps before the first result. Keep legally required consent unticked and retain its text in the count.
- Use plain words. Remove industry jargon and unsupported praise. Preserve facts, prices, warnings and required disclosures.
- Check that the next screen delivers what the button promises. End the flow when the visitor gets the result, such as a booked call.

## Handle the surface

- **Ads:** keep the real product visible in the picture. Use the platform's available button label that matches the next screen. Where the platform places its button beside the headline, use 👉 instead of 👇. Do not draw another button into the image.
- **Landing pages:** evaluate the first view on a phone. On desktop, put the picture on the left and the same words and action on the right. Repeat the same action further down.
- **Signup flows:** show one question per step and a visible trail to the result. Ask for other details after the visitor has received it.
- **App screens:** keep the list or result the user requested. Preserve the facts needed to choose. Apply the word cap to the surrounding text, with the result rows left out of the checker input.

## Check the rewrite

Save the proposed screen in the format below. Run the bundled checker from this skill directory:

```bash
python3 scripts/check.py after.txt
```

Revise until the checker exits 0. Its word and button counts are mechanical checks. Review warnings and visually check picture placement, apparent buttons and the next screen yourself. The checker cannot establish those properties from a text file.

Ask what it is, what the visitor gets and where to act again. If any answer is missing, revise. Swap in a business of a different kind: if the headline still fits, name the concrete thing being offered.

## Screen format

- `# name` labels a screen and is not counted.
- `picture:` describes the picture. Only words in double quotes count as visible text.
- `[Label]` is the one filled button; `(Link)` is small grey text.
- `{Placeholder}` is one field directly above its button.
- `<Choice>` is a tile. `---` starts the next screen.
- Every other line is visible text. The first text line is the headline.

Example input from the published checker fixtures:

```text
picture: the logo
Search the web with Google.
{Search Google}
👇
[Google Search]
```

## Report

Show BEFORE above AFTER at the same size where rendering is available. Otherwise provide both screen descriptions. Include word, button and exit counts before and after, the rules the original broke, and the three answers for the rewrite. State any property you could not verify.

## Related skills

- **copywriting:** write new copy from a brief.
- **cro:** diagnose the page beyond this screen simplification.
- **signup:** assess the wider signup flow.
