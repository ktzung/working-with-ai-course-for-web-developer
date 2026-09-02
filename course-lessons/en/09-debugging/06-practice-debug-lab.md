# Debug Lab: Fix 10 Buggy Projects

## Welcome to the Debug Lab

Theory is great, but debugging is a hands-on skill. In this lesson, you'll work through 10 intentionally buggy web projects. Each project has a specific bug that you need to find and fix using the AI-assisted debugging techniques you've learned.

## How This Works

For each project:
1. Read the bug description
2. Examine the code
3. Use AI to help diagnose the issue
4. Implement the fix
5. Verify the solution works

Time yourself for each bug. The goal is to resolve each one in under 15 minutes with AI assistance.

## Bug #1: The Infinite Loop

**Project:** React todo app
**Symptom:** App freezes when adding a new todo
**Hint:** Check the useEffect dependencies

**AI prompt to start:** "This React todo app freezes when I add a task. Here's my TodoList component. What's causing the infinite loop?"

The bug is a useEffect that updates state which triggers the same useEffect. AI will spot the dependency array issue immediately.

## Bug #2: The CORS Ghost

**Project:** Express + React blog
**Symptom:** API works in Postman but fails in the browser
**Hint:** Look at the server configuration

**AI prompt to start:** "My API returns data in Postman but the browser shows CORS errors. Here's my Express server setup."

The bug is missing CORS middleware or incorrect origin configuration. AI will explain preflight requests and fix the setup.

## Bug #3: The Silent Failure

**Project:** User authentication system
**Symptom:** Login appears to work but user data is undefined
**Hint:** Check async/await usage

**AI prompt to start:** "Login returns success but user data is undefined. Here's my auth context and login function."

The bug is a missing `await` on an async function, causing the promise to be returned instead of the resolved value.

## Bug #4: The Memory Hog

**Project:** Image gallery app
**Symptom:** Browser tab uses 2GB of memory after browsing
**Hint:** Look at event listeners and image handling

**AI prompt to start:** "My image gallery consumes massive memory. Here's my gallery component and image loading code."

The bug is event listeners added in a loop without cleanup, plus images loaded without size limits.

## Bug #5: The Race Condition

**Project:** Real-time chat application
**Symptom:** Messages appear out of order or duplicate
**Hint:** Check the WebSocket message handling

**AI prompt to start:** "Chat messages arrive out of order and sometimes duplicate. Here's my WebSocket handler and message state management."

The bug is state updates from rapid WebSocket messages overwriting each other.

## Bug #6: The Zombie Timer

**Project:** Countdown timer widget
**Symptom:** Timer continues after navigating away and coming back
**Hint:** Check useEffect cleanup

**AI prompt to start:** "My countdown timer keeps running even after I navigate away and return. Here's my Timer component."

The bug is a setInterval that's never cleared on unmount.

## Bug #7: The Form Phantom

**Project:** Multi-step form wizard
**Symptom:** Form data disappears when going back a step
**Hint:** Check state management across steps

**AI prompt to start:** "Form data is lost when navigating between steps. Here's my form wizard component."

The bug is state being reset when components unmount between steps.

## Bug #8: The API Imposter

**Project:** Weather dashboard
**Symptom:** Shows yesterday's weather even after refresh
**Hint:** Check caching and API calls

**AI prompt to start:** "My weather dashboard shows stale data. Here's my API call and data fetching logic."

The bug is aggressive browser caching of API responses without proper cache-busting.

## Bug #9: The Style Breaker

**Project:** Responsive navigation menu
**Symptom:** Menu works on desktop but overlaps content on mobile
**Hint:** Check CSS media queries and z-index

**AI prompt to start:** "My nav menu overlaps content on mobile. Here's my CSS and component structure."

The bug is missing or incorrect media queries combined with z-index issues.

## Bug #10: The Auth Bypass

**Project:** Admin dashboard
**Symptom:** Regular users can access admin routes
**Hint:** Check route protection middleware

**AI prompt to start:** "Regular users can access admin pages. Here's my route protection and auth middleware."

The bug is client-side only route protection without server-side verification.

## After the Lab

Once you've fixed all 10 bugs, reflect on the patterns:

1. Which debugging technique worked best for each type of bug?
2. How did AI help you narrow down the problem faster?
3. What common mistakes will you watch out for in your own code?

## Building Your Debug Toolkit

Ask AI to help you create a personal debugging checklist:

"Based on the 10 bugs we just fixed, create a debugging checklist I can use for future projects. Organize by category: React, API, CSS, Security."

## Key Takeaway

Debugging is a skill that improves with practice. By working through these 10 bugs with AI assistance, you've built muscle memory for common web development issues. The next time you encounter a similar problem, you'll know exactly where to look and what questions to ask AI.
