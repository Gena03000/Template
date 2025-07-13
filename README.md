/gena03000-paris-style-bags
├── rss/
│   └── nouveautes.xml
├── index.html (optionnel)
https://gena03000.github.io/gena03000-sacs-style-paris/RSS/nouveautes.xml
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Gena Campbell – Nouveautés RSS</title>
  <link rel="stylesheet" href="style.css" />
  <link rel="alternate" type="application/rss+xml"
        title="Flux RSS Gena Campbell"
        href="./RSS/nouveautes.xml" />
</head>
<body>
  <header>
    <h1>👜 Nouveautés Gena Campbell</h1>
    <p>Derniers ajouts à la collection – sacs, foulards et accessoires féminins.</p>
  </header>

  <main>
    <ul id="rss-list" class="rss-list"></ul>
  </main>

  <footer>
    <p>© 2025 Gena Campbell • <a href="./RSS/nouveautes.xml" target="_blank">Voir le flux RSS</a></p>
  </footer>

  <script src="rss-script.js"></script>
</body>
</html>
async function loadRSS() {
  try {
    const response = await fetch('./RSS/nouveautes.xml');
    const xmlText = await response.text();
    const parser = new DOMParser();
    const xml = parser.parseFromString(xmlText, 'application/xml');

    const items = xml.querySelectorAll('item');
    const list = document.getElementById('rss-list');

    items.forEach(item => {
      const title = item.querySelector('title')?.textContent || 'Sans titre';
      const link = item.querySelector('link')?.textContent || '#';
      const description = item.querySelector('description')?.textContent || '';
      const imageUrl = item.querySelector('enclosure')?.getAttribute('url');

      const li = document.createElement('li');
      li.className = 'rss-item';

      li.innerHTML = `
        <h3><a href="${link}" target="_blank">${title}</a></h3>
        ${imageUrl ? `<img src="${imageUrl}" alt="${title}" class="rss-thumbnail">` : ''}
        <p>${description}</p>
      `;

      list.appendChild(li);
    });
  } catch (error) {
    console.error('Erreur lors du chargement du flux RSS :', error);
    document.getElementById('rss-list').innerHTML =
      '<li>❌ Impossible de charger les nouveautés.</li>';
  }
}

document.addEventListener('DOMContentLoaded', loadRSS);
