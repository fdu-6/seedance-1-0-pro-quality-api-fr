# Seedance 1.0 Pro Quality API (`doubao-seedance-1-0-pro-quality`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-ad-ceramic-mug-thumb.jpg" width="820" alt="Sample video frame generated through APIMart">
</p>

> **from $0.0184 per second** on the APIMart route (billed by resolution tier) — un seul endpoint compatible OpenAI sur `https://api.apimart.ai/v1`, sans abonnement mensuel. *(relevé le 2026-10-12)*

**[Obtenir une clé API](https://go.apimart.ai/k-766b19)** · **[Tarifs en direct](https://go.apimart.ai/k-d394e1ca)** · **[Page du modèle](https://go.apimart.ai/k-eda657)** · [⚡ Démarrage en 60 secondes](#quickstart)

**Pourquoi les équipes passent par APIMart pour Seedance 1.0 Pro Quality (`doubao-seedance-1-0-pro-quality`)**

- **Une clé, tout le catalogue.** La même base URL `https://api.apimart.ai/v1` et le même en-tête `Authorization` donnent accès à Seedance 1.0 Pro Quality (`doubao-seedance-1-0-pro-quality`) et à plus de 300 autres modèles d'image, de vidéo et de langage — changez le champ `model`, pas votre client.
- **$1 minimum, paiement à l'usage.** Ni abonnement ni forfait prépayé à dimensionner à l'avance : rechargez à partir de $1 et consommez-le en appels.
- **Le montant revient dans la réponse.** Chaque tâche terminée indique le montant facturé (`cost` / `credits_cost`) ; une tâche marquée `failed` est remboursée.
- **Asynchrone par conception.** Soumettez, récupérez le `task_id`, interrogez `GET /v1/tasks/{id}` — le traitement par lots et les retries relèvent d'une file d'attente ordinaire, pas d'une intégration sur mesure.

<!-- /conv-kit:v1 -->



Seedance 1.0 Pro Quality is ByteDance's higher-fidelity video route on APIMart: text-to-video and image-to-video in 480p / 720p / 1080p, billed per generated second.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Text-to-video / image-to-video | `doubao-seedance-1-0-pro-quality` | per generated second, by resolution | see the [model page](https://go.apimart.ai/k-eda657) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/videos/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Resolution | List price | Effective price |
| --- | --- | --- |
| per generated second | — | see the live pricing page |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-d394e1ca) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | the shot description; camera and subject motion belong here |
| `duration` | integer | route default | clip length in seconds — billing is per generated second |
| `resolution` | string | route default | 480P / 720P / 1080P or the route’s own tiers |
| `size` / `aspect_ratio` | string | `16:9` | output framing |
| `image_urls` | string[] | — | first frame, first + last frame, or style references, where supported |
| `seed` | integer | — | fix the seed to reproduce a take |

The table lists the fields accepted by the APIMart `videos/generations` endpoint. Check the [model page](https://go.apimart.ai/k-eda657) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/videos/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"doubao-seedance-1-0-pro-quality","prompt":"A bamboo forest at sunrise, slow push-in","duration":5,"resolution":"720P","size":"16:9"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/videos/generations", headers=HEADERS, timeout=60, json={
    "model": "doubao-seedance-1-0-pro-quality", "prompt": "A bamboo forest at sunrise, slow push-in", "duration": 5,
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["videos"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**Quel est l'id du modèle Seedance 1.0 Pro Quality ?**

`doubao-seedance-1-0-pro-quality`. Voir le tableau des routes ci-dessus.

**Comment la facturation fonctionne-t-elle ?**

**from $0.0184 per second** on the APIMart route (billed by resolution tier). La tâche terminée indique le montant exact dans son champ `cost` ; une tâche échouée est remboursée.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-766b19) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [seedance-1-0-pro-quality-prompts-fr](https://github.com/fdu-6/seedance-1-0-pro-quality-prompts-fr)
- **Same model, other languages** — 9 more language builds: 中文 [seedance-1-0-pro-quality-api-cn](https://github.com/lo0-trmv3dup2/seedance-1-0-pro-quality-api-cn) · Deutsch [seedance-1-0-pro-quality-api-de](https://github.com/qt89oz47ugft9i/seedance-1-0-pro-quality-api-de) · Español [seedance-1-0-pro-quality-api-es](https://github.com/g708boud1/seedance-1-0-pro-quality-api-es) · Relay/gateway [seedance-1-0-pro-quality-api-gateway](https://github.com/bzo1u6sfkle/seedance-1-0-pro-quality-api-gateway) · Bahasa [seedance-1-0-pro-quality-api-id](https://github.com/i74lkq1472/seedance-1-0-pro-quality-api-id) · 日本語 [seedance-1-0-pro-quality-api-ja](https://github.com/fkqozxa1p/seedance-1-0-pro-quality-api-ja) · 한국어 [seedance-1-0-pro-quality-api-ko](https://github.com/t26b6qupjc93k7/seedance-1-0-pro-quality-api-ko) · Português [seedance-1-0-pro-quality-api-pt](https://github.com/onpfo05pvnb/seedance-1-0-pro-quality-api-pt) · Русский [seedance-1-0-pro-quality-api-ru](https://github.com/rb5mqsn/seedance-1-0-pro-quality-api-ru)
- **More models in this language** — [midjourney-api-fr](https://github.com/apimartt1q/midjourney-api-fr) · [gemini-2-5-flash-image-preview-api-fr](https://github.com/haftslut7ha/gemini-2-5-flash-image-preview-api-fr) · [nano-banana-api-fr](https://github.com/f84q06w110/nano-banana-api-fr) · [gemini-3-1-flash-image-preview-api-fr](https://github.com/ok71ckyc/gemini-3-1-flash-image-preview-api-fr) · [nano-banana-2-api-fr](https://github.com/ms0p12t/nano-banana-2-api-fr) · [gemini-3-pro-image-preview-api-fr](https://github.com/w-44vba/gemini-3-pro-image-preview-api-fr)
- **Topic hubs** — every build of this model, all languages: [seedance-1-0-pro-quality](https://github.com/topics/seedance-1-0-pro-quality) · [seedance-1-0-pro-quality-api](https://github.com/topics/seedance-1-0-pro-quality-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-eda657> | `apimart.ai/fr/model/doubao-seedance-1-0-pro` |
| Live pricing | <https://go.apimart.ai/k-d394e1ca> | `apimart.ai/fr/pricing` |
| Get an API key | <https://go.apimart.ai/k-766b19> | `apimart.ai/fr/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Seedance 1.0 Pro Quality is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
