<div align="center">
    <h1>Whisper on Fly GPUs</h1>
    <p>Run <strong><a href="https://github.com/openai/whisper">OpenAI Whisper</a></strong> as a <strong><a href="https://github.com/replicate/cog">Replicate Cog</a></strong> on Fly.io!</p>
</div>

![cog](https://github.com/fly-apps/cog-whisper/assets/3727384/4d757f88-29c1-4966-ace9-01d0b1a630ac)

This app exposes the Whisper model via a simple HTTP server, thanks to Replicate Cog. Cog is an open-source tool that lets you package machine learning models in a standard, production-ready container. When you're up and running, you can trascribe audio using the /predictions endpoint.

## Launch

Create a deploy the app in one single command:

```bash
fly launch --from https://github.com/fly-apps/cog-whisper --no-public-ips
```

Assign a [Flycast](https://fly.io/docs/networking/private-networking/#flycast-private-load-balancing) IP to the app:

```bash
fly ips allocate-v6 --private
```

That's it! You can now access the app at `http://<APP_NAME>.flycast/predictions`

> [!IMPORTANT]  
> By default, the app runs on Fly GPUs — Nvidia L40s to be exact. This can be customized in the fly.toml `vm` settings. It will run on a standard Fly Machine — but performance will be reduced.

## Usage

```bash
curl -X PUT \
     -H "Content-Type: application/json" \
     -d '{
           "input": {
             "audio": "https://fly.storage.tigris.dev/cogs/bun_on_fly.mp3"
           }
         }' \
     http://cog-whisper.flycast/predictions/test | jq

```

## Local Setup

1. Clone the `cog-whisper` repository from GitHub:

    ```bash
    git clone git@github.com:fly-apps/cog-whisper.git
    ```

2. Navigate into the cloned directory:

    ```bash
    cd cog-whisper
    ```
    
3. Run local predictions. First, run `get_weights.sh` from the project root to download pre-trained weights:
    ```bash
    ./scripts/get_weights.sh:
    ```
  You can then build a container and run predictions like so:
    ```bash
    cog predict -i audio="<path/to/your/audio/file>"
    ```

4. Build the Docker image using `cog`:

    ```bash
    cog build -t whisper
    ```

## Having trouble?

Create an issue or ask a question here: https://community.fly.io/