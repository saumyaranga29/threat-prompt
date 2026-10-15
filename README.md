# Business logic threat model prompt

A reusable prompt that reads a free-text system description and returns 10
business logic threats as JSON.

## Layout

- prompt/system_prompt.txt: system prompt
- prompt/user_prompt.txt: user prompt template ({system_description} is the input slot)
- scenarios/payloop.txt: the PayLoop scenario
- outputs/: raw model output, unedited (two separate runs)
- NOTE.md: short write-up

## How I ran it

I pasted the system prompt into a new chat, then sent the user prompt with
{system_description} replaced by the PayLoop text. I did this twice in
separate chats. The replies are saved as they came back.
