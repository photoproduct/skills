---
name: photoproduct
description: "Create images, video, music, voiceovers and 3D with Photoproduct.io's generation models through its MCP server. Use it whenever the user wants a picture, a product photo, a character, an outfit, a character sheet, a scene, a video clip, an animated photo, music, a voiceover, an upscale or a 3D model, or wants to turn a local file or a public link into one of those."
---

# Photoproduct.io

Photoproduct.io runs the user's choice of image, video, audio and speech models and
pays for each run from the credits of the user's own Photoproduct.io account. Work
through the connected `photoproduct` MCP tools; the server's tool descriptions and
input schemas are the source of truth for models, params and prices. If the tools are
not connected yet, point the user to https://photoproduct.io/agents.

## Ground rules

- Browsing models, reading a prompt guide or estimating a cost never authorizes a
  generation. Call `run` only for a generation the user asked for or clearly confirmed.
- Keep the user's brief: subject, count, format, duration and any model they named.
  Use sensible defaults for the rest and ask only for inputs a run cannot do without.
- Stay inside the user's budget. Before an expensive run (video, several images,
  long audio) state the model and its credit price from `list_models`, and wait for a
  yes unless the user already agreed to it. Each connection also has a daily limit.
- Safety filters apply. If a prompt is refused, say so and offer a different idea;
  never reword a request to get around a filter, and never depict a real, identifiable
  person without their consent.

## Pick the workflow

For anything with characters, scenes or video, call `prompt_guide` with the task below
first and follow the structure it returns; it names the models that suit the task and
the mistakes the structure prevents.

| The user wants | Workflow |
|---|---|
| Product photos from their own product photo | Product photos (below) |
| A new original character | `prompt_guide` task `face_lock`: one neutral portrait that fixes the face |
| A new look on that character | `prompt_guide` task `character_outfit` |
| Front, back and face of a character in one frame | `prompt_guide` task `character_sheet` |
| A cinematic environment or establishing still | `prompt_guide` task `scene_plate` |
| A video from a description | `prompt_guide` task `video_scene` |
| An image brought to life | `prompt_guide` task `video_from_image` |
| A clip between a first and a last frame | `prompt_guide` task `video_keyframes` |
| The fixed facts of a series or story | `prompt_guide` task `story_bible` |
| To find or compare models | `list_models` with their words as `search`; show a short comparison |

## Product photos

1. Get the photo in as an `upload_ref`: a local file with `upload_file` (base64, for
   small images) or a public link with `upload_from_url`.
2. Call `list_models` with `search` such as "edit" or "product" and pick an image model
   that takes an image input (its `file_inputs` say which param).
3. Write one prompt per look: the product exactly as photographed, the surface, the
   light, the background and the camera. Keep the product's shape, colours, label and
   text unchanged.
4. Run once per look, or once with the model's image count for variations of one look.

## Running and following a job

1. Choose the model with `list_models`. Use only params its entry lists; never invent a
   model name or a param.
2. File inputs take an `upload_ref` (`upload_file` for small local files,
   `upload_from_url` for public links and anything large such as video), or the
   `job_ref` of an earlier finished job to build on its result.
3. Call `run`. It answers at once with a `job_ref`. In a chat app that shows the
   Photoproduct card, the card shows the progress and the files.
4. Check with `get_job`. Images usually take seconds, video one to a few minutes. Say a
   result is ready only when `get_job` reports it completed.
5. If a `run` call fails without an answer, call it again with the same
   `idempotency_key`; never start a second generation for the same request.
6. In a terminal agent, when the user wants the files locally, download every URL in
   `outputs` (or `output_url` for a single file) into the folder they name.
7. A failed run is not charged; tell the user and suggest a change. For the next step
   on a result (animate it, change the outfit, upscale), pass the earlier `job_ref` as
   the file input.
