## Poetry Entry System

Render link: TODO

The Poetry Entry System lets you keep a list of Arabic poems. For each poem you enter the author, title, maqam(s), form, the author's birth year, and optional notes. The server then works out three extra fields: the era (Classical or Modern, based on the birth year), the mood (based on the maqams), and whether the poem uses more than one maqam. Anyone can open the page, but you log in with GitHub to see and save poems, and you only see and change your own. The first time you log in, a new account is made for you with three example poems.

- **Goal:** turn my A2 poetry app into a two-tier app with Express, MongoDB, user accounts, and a CSS framework.
- **Challenges:**
  - Moving from A2's in-memory array with number ids to MongoDB's `_id`, which meant changing the front end in several places.
  - Editing poems broke after the move. My fetch calls didn't send a `Content-Type: application/json` header, so `express.json()` never parsed the body.
  - Getting GitHub OAuth to work both locally and on Render, since each needs its own callback URL.
- **Authentication:** GitHub OAuth using passport and passport-github2. The course staff all have GitHub accounts, and the app never has to store passwords.
- **CSS framework:** Pico.css. It styles plain HTML elements without needing lots of classes, which kept my A2 HTML mostly the same.
  - My own CSS in `main.css`:
    - Limits the form to 500px wide.
    - Puts the checkboxes and radio buttons side by side.
    - Uses smaller padding on the Edit and Delete buttons.
    - Makes the Delete button red with black text.

## Technical Achievements
- **Tech Achievement 1 (10 points): OAuth with GitHub.** Users log in with their GitHub account using passport and the passport-github2 strategy. The GitHub username is saved in the session and used to find that user's poems in MongoDB. The first time someone logs in, a user record and three example poems are created, and an alert tells them a new account was made. This was hard because OAuth needs a registered GitHub OAuth app, a callback route, and different callback URLs for localhost and Render.
- **Tech Achievement 2 (5 points): Express middleware.** I used these five npm middleware packages:
  - **cookie-session:** stores the logged-in username in a signed cookie, so the server knows who is making each request.
  - **passport:** handles login; `passport.initialize()` and `passport.authenticate()` run the GitHub OAuth flow.
  - **helmet:** adds security-related HTTP headers, like a Content-Security-Policy, to every response.
  - **compression:** gzips responses so pages load faster.
  - **morgan:** logs each request's method, URL, status, and response time to the console.

### Design/Evaluation Achievements
- **Design Achievement 1 (10 points): W3C accessibility.** I followed these twelve tips from the W3C Web Accessibility Initiative:
  1. **Provide informative, unique page titles.** I changed the title from A2's "CS4241 Assignment 2: Poetry Entry Syrian" to "Your Poems - Poetry Entry System", which says what the page is for and which site it belongs to.
  2. **Use headings to convey meaning and structure.** Each page has one `h1`. The main page has `h2` headings for "Add or Edit a Poem" and "Your Poems".
  3. **Make link text meaningful.** The login link says "Log in with GitHub" instead of something like "click here".
  4. **Provide clear instructions.** The form says which fields are required. The maqam group says to check every maqam the poem uses. The birth year field has a hint with an example year and explains how the era is decided.
  5. **Provide sufficient contrast between foreground and background.** The Delete button uses black text on red (about 5.3:1). I didn't use white text, because white on red is only about 4:1 and fails.
  6. **Don't use color alone to convey information.** The Delete button is red but also says "Delete". Required fields are marked with the word "(required)" instead of a color or symbol.
  7. **Ensure that form elements include clearly associated labels.** Every text input and the textarea has a `label` with a matching `for` and `id`. The checkboxes and radio buttons are wrapped in labels and grouped in a `fieldset` with a `legend`.
  8. **Provide easily identifiable feedback.** After adding, editing, or deleting a poem, a status message says "Poem added.", "Poem updated.", or "Poem deleted.". If no maqam is checked, it says so instead of sending the form.
  9. **Create designs for different viewport sizes.** I added a viewport meta tag to both pages and used Pico's `container` class, so the text and form fields resize to fit a phone screen instead of showing a zoomed-out desktop page.
  10. **Use markup to convey meaning and structure.** The pages use a `main` element, and the table headers use `scope="col"`. The status message uses `role="status"` so screen readers announce it. Each Edit and Delete button has an `aria-label` with the poem's title, so they aren't all just "Edit" and "Delete".
  11. **Help users avoid and correct mistakes.** Required fields use the `required` attribute. The birth year only accepts years from 1 to 2026. Deleting a poem asks for confirmation first.
  12. **Ensure that all interactive elements are keyboard accessible.** All actions are real `button` elements or links, so they work with Tab and Enter. Clicking Edit moves keyboard focus to the Author field, so you can start editing right away.
