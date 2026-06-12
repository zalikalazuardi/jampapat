# jampapat
all information about Jampapat class
from pathlib import Path
import fitz

pdf_path = "/mnt/data/informasi tanaman di taman jampapat"
outdir = Path("/mnt/data/taman_jampapat_web")
outdir.mkdir(exist_ok=True)

pdf = fitz.open(pdf_path)
plants = [
    ("Bromelia Giant","Alcantarea imperialis"),
    ("Andong Merah","Cordyline fruticosa"),
    ("Pandan Bali","Yucca / Dracaena"),
    ("Pohon Pinang","Areca catechu"),
    ("Agave Attenuata","Agave attenuata"),
    ("Calathea Lutea","Calathea lutea"),
    ("Philodendron Burle Marx","Philodendron burle marx"),
    ("Iris Kuning","Iris pseudacorus"),
]

cards = []
for i,(name,sci) in enumerate(plants):
    pix = pdf[i].get_pixmap(matrix=fitz.Matrix(2,2), alpha=False)
    img_name=f"plant_{i+1}.png"
    pix.save(str(outdir/img_name))
    cards.append((name,sci,img_name))

html = f"""
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Taman Jampapat</title>
<style>
body{{font-family:Arial,sans-serif;margin:0;background:#f5faf5}}
header{{background:linear-gradient(135deg,#2e7d32,#66bb6a);color:white;padding:60px 20px;text-align:center}}
.grid{{display:grid;grid-template-columns:repeat(auto-fit,minmax(280px,1fr));gap:20px;padding:30px;max-width:1200px;margin:auto}}
.card{{background:white;border-radius:16px;overflow:hidden;box-shadow:0 4px 12px rgba(0,0,0,.12)}}
img{{width:100%;height:260px;object-fit:cover}}
.content{{padding:16px}}
h3{{margin:0;color:#2e7d32}}
small{{color:#666}}
</style>
</head>
<body>
<header>
<h1>🌿 Informasi Tanaman Taman Jampapat</h1>
<p>Website digital berdasarkan PDF katalog tanaman.</p>
</header>
<div class="grid">
"""
for name,sci,img in cards:
    html += f'<div class="card"><img src="{img}"><div class="content"><h3>{name}</h3><small><i>{sci}</i></small></div></div>'
html += "</div></body></html>"

(outdir/"index.html").write_text(html, encoding="utf-8")
print(str(outdir/"index.html"))
