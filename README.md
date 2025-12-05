AutoPost – n8n Workflow README

Overview
--------
This workflow automatically pulls the latest post data from a Google Sheet, downloads the associated image, uploads it to the GetLate API, generates tweet content, and publishes a post to X (Twitter).

It is designed for scheduled automated posting and supports multi-part threads.

---------------------------------------------------
1. Input & Media Processing (Left Panel – Blue)
---------------------------------------------------

1. Schedule Trigger
   - Runs every 8 hours (0 */8 * * *)
   - Starts the workflow automatically.

2. Get Topic (Google Sheets)
   - Reads the latest row from a Google Sheet.
   - Fetches:
       - Headline
       - Article URL
       - Image URL
       - Tweet text
       - Hashtags
       - Tweet2 (optional)
   - Uses Google Sheets OAuth2 credentials.

3. Get Img
   - Downloads the image from the provided Image URL.

4. Rename Img
   - Renames the downloaded file using format:
       image-YYYY-MM-DD_HHMMSS.ext
   - Ensures uniqueness before upload.

5. Upload IMG
   - Uploads the renamed image to the GetLate API.
   - Returns a hosted media URL.
   - Uses Bearer Token authentication.

---------------------------------------------------
2. Posting to X.com (Right Panel – Green)
---------------------------------------------------

6. Set Tweets
   - Creates:
       - tweet (main tweet with headline, media URL, and hashtags)
       - tweet2 (thread reply)
   - Text is dynamically generated from the spreadsheet and uploaded image URL.

7. If (Tweet Length Check)
   - Ensures both tweets are <= 280 characters.
   - Only valid tweets pass through.

8. Post Twitter
   - Sends a POST request to https://getlate.dev/api/v1/posts
   - Includes media, tweet text, and thread items.
   - publishNow: true

9. No Operation
   - Used to maintain a clean flow or for future extension.

---------------------------------------------------
Credentials
---------------------------------------------------

The workflow references credential IDs for:
- Google Sheets OAuth2
- HTTP Bearer Token Auth

Actual secret values are NOT exported in the JSON file.

---------------------------------------------------
How to Test the Workflow
---------------------------------------------------

Manual Test
-----------
1. Open workflow.
2. Click "Execute workflow".
3. Observe each node:
   - Image downloads
   - Image uploads
   - Tweets generate correctly
   - Post is created on X.com

Testing Node-by-Node
--------------------
Execute nodes individually in this order:
1. Get Topic
2. Get Img
3. Rename Img
4. Upload IMG
5. Set Tweets
6. Post Twitter

Testing With Sample Row
-----------------------
- Add a valid image URL in your sheet.
- Ensure all required fields are filled.
- Run workflow manually.

---------------------------------------------------
Common Errors & Fixes
---------------------------------------------------

Invalid URL
-----------
Occurs when:
- Image URL is empty.
- The sheet contains empty rows.
Fix: Remove empty rows or filter before Get Img.

Tweet Length Error
------------------
Triggered if tweet > 280 characters.
Fix: Shorten tweet or adjust formatting.

---------------------------------------------------
Notes
---------------------------------------------------

- Workflow is safe to share (no API keys included).
- Google Sheet should be private unless intentionally shared.
- API tokens must be re-added after importing workflow.

