<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Каталог Аниме</title>
    <style>
        body { font-family: sans-serif; background: #0f172a; color: #fff; margin: 0; padding: 20px; }
        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 20px; }
        .card { background: #1e293b; padding: 10px; border-radius: 8px; }
        .card img { width: 100%; height: 300px; object-fit: cover; border-radius: 4px; }
        .controls { display: flex; gap: 10px; margin-bottom: 20px; justify-content: center; }
        input, select, button { padding: 10px; border-radius: 5px; border: none; }
        button { background: #6366f1; color: white; cursor: pointer; }
    </style>
</head>
<body>

<div class="controls">
    <input type="text" id="searchInput" placeholder="Поиск аниме...">
    <select id="genreSelect">
        <option value="">Все жанры</option>
        <option value="22">Романтика</option>
        <option value="37">Мистика</option>
    </select>
    <button onclick="loadAnime()">Найти</button>
</div>

<div class="grid" id="animeGrid"></div>

<script>
    async function loadAnime() {
        const search = document.getElementById('searchInput').value;
        const genre = document.getElementById('genreSelect').value;
        const grid = document.getElementById('animeGrid');
        grid.innerHTML = 'Загрузка...';

        let url = `https://api.jikan.moe/v4/anime?limit=25&order_by=popularity&sort=asc`;
        if (search) url += `&q=${encodeURIComponent(search)}`;
        if (genre) url += `&genres=${genre}`;

        try {
            const res = await fetch(url);
            const data = await res.json();
            grid.innerHTML = '';
            
            data.data.forEach(anime => {
                const card = document.createElement('div');
                card.className = 'card';
                card.innerHTML = `
                    <img src="${anime.images.jpg.large_image_url}" alt="${anime.title}">
                    <h3>${anime.title}</h3>
                    <p>Рейтинг: ${anime.score || 'N/A'}</p>
                `;
                grid.appendChild(card);
            });
        } catch (e) {
            grid.innerHTML = 'Ошибка загрузки данных.';
        }
    }
    loadAnime();
</script>

</body>
</html>
