# Colab
- Colab: <https://colab.research.google.com/>
- Colab on GitHub: <https://github.com/googlecolab/colabtools>

## Shortcuts
- Show shortcuts: `ctrl + m, h`
- Insert code cell above: `ctrl + m, a`
- Insert code cell below `ctrl + m, b`

## Download Files from Colab
```python
from google.colab import files
files.download('<file_to_download>')
```

## Upload Files to Colab
```python
from google.colab import files
uploaded = files.upload()
```

## Mount Google Drive
```python
from google.colab import drive
drive.mount('/gdrive')
```

```bash
ls -la /gdrive
```
