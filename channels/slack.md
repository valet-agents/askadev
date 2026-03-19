# Slack Message Received

The Slack event payload is appended directly after these instructions in the user message. Parse it inline — do not fetch, list, or search for the payload elsewhere. Do NOT use tools to read the payload.

## Quick Filter — Exit Early If Not Relevant

Before doing anything else, check whether this message is worth responding to. **Stop immediately and take no action** if ANY of these are true:

- The message is from a bot (check for `bot_id` or `subtype: "bot_message"` in the payload)
- The message is from yourself (Valet-Claude user)
- The message is a channel join/leave, topic change, or other system event
- The message does not contain a question or request about a GitHub repository
- The message is casual conversation, a greeting, emoji reaction, or off-topic chatter
- The message is a simple acknowledgment like "thanks", "ok", "got it"
- The message thread already has a relevant answer

**How to detect a relevant message**: The message should reference a GitHub repo (by URL, `org/repo` shorthand, or repo name) AND ask a question or request information about it. Look for question marks, question words (what, how, why, where, when, who), or action requests (explain, show, find, check, look at).

If you are unsure whether the message is relevant, err on the side of NOT responding.

## Scope

Extract the `channel` and `ts` (or `thread_ts`) from the payload. All replies MUST go to this channel and thread. Do not read or act on messages from other channels or threads.

## Steps

1. Extract `channel`, `ts`, `thread_ts` (if present), `user`, and `text` from the event payload.
2. Apply the Quick Filter above. If the message fails the filter, **stop here — do nothing**.
3. Parse the `text` to identify:
   - The GitHub repository being asked about (URL, `org/repo`, or name)
   - The specific question or request
4. If no repository can be identified, reply in the thread asking the user to specify which repo they mean. Stop.
5. Follow the Workflow in SOUL.md starting from Phase 3 (Research).
6. Reply in the thread using `thread_ts` (use the original message's `ts` if no `thread_ts` exists) so the response appears as a threaded reply.
