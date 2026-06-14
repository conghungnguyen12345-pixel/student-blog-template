---
title: Reflecting on Critique Canvas — What We Built, What We Learned
date: 2026-06-09
author: Hung Nguyen
summary: A concluding reflection on the performance, user experience, accessibility and functional results of the Critique Canvas web application prototype
tags: [reflection, evaluation, accessibility, performance, lessons]
---

## Introduction

This post is a reflection on the fully completed Critique Canvas prototype, a platform that was designed for artists who want their work to receive accurate and pinpointed critique. My earlier posts were focused on planning and decision-making. This post will showcase what was actually built, assess its performance, usability, and how well it fulfilled the original requirements. The evaluation is based on a Lighthouse audit, a WCAG AA checklist, keyboard navigation testing, and a friend user testing session conducted before submission.

## Performance and Technical Behaviour Evaluation

To objectively analyse the performance of the platform, I ran a Lighthouse audit on the homepage using Chrome DevTools in desktop mode. The results were:

| Category | Score |
|---|---|
| Performance | 97 |
| Accessibility | 96 |
| Best Practices | 74 |
| SEO | 100 |

I did a testing trial and uploaded an image. The speed was fast, similar to a normal online platform. The gallery page also operated properly. Lighthouse results back this up with First Contentful Paint of 1.0 seconds, Largest Contentful Paint of 1.0 seconds, Total Blocking Time of 0 milliseconds and Speed Index of 1.0 seconds, which are strong results for a newly constructed prototype.

Lighthouse identified three crucial issues. The first is render-blocking requests with estimated savings of 770 milliseconds. The Google Fonts import loads before the page renders, slowing First Contentful Paint. This reflects a planning gap where our team prioritised visual aesthetic over load performance. In production, this could be fixed by using font-display: swap or self-hosting the fonts. In addition to that, 41 KiB of unused CSS and 108 KiB of unused JavaScript were detected because global stylesheets and libraries are loaded on all pages rather than on demand.

The lowest score is the Best Practices score of 74, since the software is mostly running over HTTP instead of HTTPS. This is expected for a localhost prototype, not a fundamental design fault. The documented browser failures to console were also helpful in that they showed edge circumstances the annotation canvas JavaScript wasn’t handling, and that will need to be rectified before any meaningful deployment.

The strong performance profile reflects an intentional decision made during planning. Our team chose server-side rendering with HTMX partial updates rather than a heavy client-side framework, therefore ensuring our page responds to user interaction without requiring a reload. The near-zero blocking time validates this choice.

## User Experience and Accessibility Evaluation

I also conducted a user testing session with a friend to evaluate the user experience. The participant was chosen for her computing background, experience in web development and shared design coursework, therefore ensuring the feedback was technically informed. She was not given any instructions and was asked to complete five tasks: create a profile, upload an artwork, find it in the gallery, leave an annotation, and use the filter pills. All five tasks were completed in approximately two to two and a half minutes without assistance.

The annotation feature fared especially well. My friend said the instructions were simple and easy to follow, and that the crosshair cursor made it immediately obvious that clicking the image would set a pin. The filter pills were equally intuitive. My friend observed that when someone sees a dropdown with categories, their mind naturally goes to understanding that this is a filter feature, which is consistent with how we designed it.

Two usability problems were found. The first problem was that you could not delete an annotation. My friend tried to erase their review, but it wouldn’t work. This shows there was a gap in our original planning. We spent so much time designing the pin and feedback features to work correctly that we forgot about deletion. But that should have been a given from the outset. Second, the Story Chain feature was confusing as it required one person to submit before the next could progress. I was largely focused on Critique Canvas and Collab Roulette and didn't spend as much time testing Story Chain; therefore, this was not noticed during development.

Only one person was tested, which limits the accuracy of these findings. In a larger-scale evaluation, three to five users with a high level of knowledge, including practising artists with no technical background, would be included to better represent the platform's intended audience.

For accessibility, our team conducted a WCAG AA checklist and keyboard navigation test:

| WCAG Criterion | Result |
|---|---|
| Images have alt text | Pass |
| Form inputs have labels | Pass |
| Tab navigation visible | Pass, orange outline on all elements |
| Skip link present | Pass, appears on first Tab press |
| Colour contrast sufficient | Partial fail, grey text flagged by Lighthouse |

Keyboard navigation testing showed that login, gallery navigation and opening an artwork can all be done using only Tab and Enter. However, the annotation form’s category dropdown and submit button were not keyboard accessible; therefore, a keyboard-only user could not complete the entire annotation workflow without a mouse.

The grey writing that fails the contrast test is a hint that something is wrong with how we prepare for design. The dark purple look was thought up by our team, and the contrast issue only arose once Lighthouse selected it in the review. Contrast ratios should have been a design constraint from the start, not something we learned at the end.

## Critical Reflection and Improvement Planning

The annotation system was much harder to use than we thought. The original idea was to allow users to place pins on an artwork, but I realised that saving annotation positions as pixel coordinates was an issue for varying screen sizes. A pin saved at 340 px on a 1200px-wide screen will be in a totally different place on a 600px-wide screen. To solve this problem, I modified the system to save coordinates as percentages of the image size, which necessitated adjustments to both the frontend and backend logic. As said in previous entries, this decision dictated our entire data model, and is why our annotations table holds x and y as REAL values, not integers.

The second improvement I would prioritise is keyboard accessibility of the annotation form. The popup appears dynamically after a canvas click, but focus is not programmatically moved into it, making the dropdown and submit button unreachable via Tab. The fix is to use JavaScript to set focus on the first form element when the popup opens, which would ensure the core annotation workflow reaches WCAG AA compliance.

If I were to start this project over, I would build a moodboard to highlight the style and aesthetic of the platform, since I believed the design and the colour combination needed more time to evolve. I would also be more hands-on with all additional features rather than focusing primarily on Critique Canvas and Collab Roulette, therefore ensuring that every feature receives equal testing attention before submission.

## Retrospective Assessment of Functional Requirements

In my initial post, I identified two tiers of requirements. The core tier with features like artwork upload, click-to-annotate, annotation pins, accessible fallback list and user-linked annotations has been fully implemented, and operated well during testing. It was based on the same logic as other online platforms, which made it easy for users to understand. The optional tier included category tagging, filtering and deletion. Tagging and filtering were both implemented, which exceeded the original minimum scope. Deletion was not implemented, and user testing confirmed this is a crucial gap that users want addressed.

Deletion is a crucial example of a misunderstood demand, since it was something that users said they wanted right away. This shows our technology-driven motivation and our lack of consideration for user needs. In future iterations, deletion would be an essential factor from the beginning.

In addition to the original design, our team added two features: Collab Roulette and Story Chain. These were important as they give our users a deeper experience. But having two more features limited our testing capacity. Story Chain was not fully tested before submission; therefore, the issue was only identified during user testing, not development. This reflects a lesson in scope management, where high-quality scope management is to deliver a solution that is non-complex and can successfully solve problems. Additional features around the main attribute also require investment in testing to avoid confusion.

## Conclusion

Critique Canvas successfully delivered its core value as a platform where artists can receive spatially anchored, pinpointed feedback on their work. The Lighthouse audit, WCAG checklist, keyboard navigation test, and friend user testing all confirm that the platform performs well. The crucial areas for improvement are keyboard focus in the annotation popup, annotation deletion, colour contrast for secondary text, and the Story Chain submission flow. These are all achievable and would meaningfully raise the quality of the platform in a future iteration.

