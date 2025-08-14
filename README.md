# RLBot Central Wiki

The RLBot Wiki is available at <https://wiki.rlbot.org/>.

The RLBot Wiki primarily houses resources for bot development, but you can also find guides for users, framework documentation, and community insights.

## Setup

For linux (windows is probably very similar):

```bash
python -m venv venv # Create virtual env for python
source venv/bin/activate # Activate virtual env
pip install -r requirements.txt # Install mkdocs
```

Refer to `mkdocs --help` for serving locally, building and manual deployment with [MkDocs](https://www.mkdocs.org/).

## Deployment

The `main` branch is automatically deployed. See Github actions.
