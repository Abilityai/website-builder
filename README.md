# Website Builder

A Trinity agent that builds and runs websites from a plain-language brief.

1. In your Trinity instance, open **Library**, pick **Website Builder**, **Create**.
2. Add a `VERCEL_TOKEN` (and a GitHub token, unless the agent already has a GitHub PAT).
3. Optional, recommended: **Library → Skills**, assign `create-website` to the agent so it always uses the latest released builder.
4. In chat: *"Build a site for Acme Bakery: home, menu, contact, warm style."* You get a live URL.
5. Later: *"acme-bakery: change the headline to …"*, and it edits and redeploys.

Every site gets its own private GitHub repo and Vercel project. Nothing is invented: details the brief leaves out are marked `TBD` and listed.

Built on the `create-website` skill from the [Ability.ai marketplace](https://github.com/Abilityai/abilities) / [trinity-skills](https://github.com/Abilityai/trinity-skills).
