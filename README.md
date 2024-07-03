<div align="center">
    <h1>Whisper on Fly GPUs</h1>
    <p>Run OpenAI Whisper as a Replicate Cog on Fly.io!</p>
</div>

![cog](https://github.com/fly-apps/cog-whisper/assets/3727384/4d757f88-29c1-4966-ace9-01d0b1a630ac)

This app exposes the Whisper model via a simple HTTP server, thanks to Replicate Cog. Cog is an open-source tool that lets you package machine learning models in a standard, production-ready container. When you're up and running, you can trascribe audio using the /predictions endpoint.

## Launch

```bash
fly launch --from https://github.com/fly-apps/cog-whisper --no-public-ips
```

Assign a [Flycast](https://fly.io/docs/networking/private-networking/#flycast-private-load-balancing) IP to the app
```bash
fly ips allocate-v6 --private
```

That's it! You can now access the app at `http://<APP_NAME>.flycast/predictions`

## Usage

```curl
curl -X PUT \
     -H "Content-Type: application/json" \
     -d '{
           "input": {
             "audio": "https://fly.storage.tigris.dev/cogs/bun_on_fly.mp3"
           }
         }' \
     http://cog-whisper.flycast/predictions/test | jq

```