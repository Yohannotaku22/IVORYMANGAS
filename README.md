# https://github.com/Ivoirotaku/Ivoirotaku.github.io
Site de Mangas Africain 
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Accueil - MangaCI</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <h1>Bienvenue sur MangaCI</h1>
        <nav>
            <ul>
                <li><a href="index.html">Accueil</a></li>
                <li><a href="mangas.html">Mangas</a></li>
                <li><a href="post.html">Publier</a></li>
                <li><a href="about.html">À propos</a></li>
                <li><a href="contact.html">Contact</a></li>
            </ul>
        </nav>
    </header>
    <main>
        <section class="intro">
            <h2>Découvrez des Mangas Éblouissants</h2>
            <p>Explorez une vaste collection de mangas créés par des talents émergents.</p>
        </section>
        <section class="gallery" id="gallery">
            <!-- Les mangas seront ajoutés ici -->
        </section>
    </main>
    <footer>
        <p>&copy; 2025 MangaCI. Tous droits réservés.</p>
    </footer>
    <script src="script.js"></script>
</body>
</html>
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Publier un Manga - MangaCI</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <h1>Publier un Manga</h1>
        <nav>
            <ul>
                <li><a href="index.html">Accueil</a></li>
                <li><a href="mangas.html">Mangas</a></li>
                <li><a href="post.html">Publier</a></li>
                <li><a href="about.html">À propos</a></li>
                <li><a href="contact.html">Contact</a></li>
            </ul>
        </nav>
    </header>
    <main>
        <section class="upload-manga">
            <h2>Publier un Manga</h2>
            <form id="uploadForm">
                <label for="title">Titre du manga :</label>
                <input type="text" id="title" name="title" required>
                
                <label for="description">Description :</label>
                <textarea id="description" name="description" required></textarea>
                
                <label for="cover">Télécharger la couverture :</label>
                <input type="file" id="cover" name="cover" accept="image/*" required>
                
                <label for="file">Télécharger le manga (PDF) :</label>
                <input type="file" id="file" name="file" accept=".pdf" required>
                
                <button type="submit">Publier</button>
            </form>
            <div id="uploadStatus"></div>
        </section>
    </main>
    <footer>
        <p>&copy; 2025 MangaCI. Tous droits réservés.</p>
    </footer>
    <script src="script.js"></script>
</body>
</html>
body {
    font-family: 'Arial', sans-serif;
    margin: 0;
    padding: 0;
    background-color: #f4f4f4;
    color: #333;
}

header {
    background-color: #333;
    color: #fff;
    padding: 1rem 0;
    text-align: center;
}

nav ul {
    list-style: none;
    padding: 0;
}

nav ul li {
    display: inline;
    margin: 0 1rem;
}

nav ul li a {
    color: #fff;
    text-decoration: none;
}

.intro {
    background-color: #e2e2e2;
    padding: 2rem;
    text-align: center;
}

.gallery {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    padding: 2rem;
}

.manga-card {
    background-color: #fff;
    border: 1px solid #ddd;
    border-radius: 5px;
    margin: 1rem;
    padding: 1rem;
    width: 200px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.manga-card img {
    max-width: 100%;
    border-bottom: 1px solid #ddd;
    padding-bottom: 1rem;
}

.comments {
    border-top: 1px solid #ddd;
    padding-top: 1rem;
    margin-top: 1rem;
}

.comment-form {
    display: flex;
    flex-direction: column;
    margin-bottom: 1rem;
}

.comment-form input, .comment-form button {
    padding: 0.5rem;
    margin-bottom: 0.5rem;
    border: 1px solid #ddd;
    border-radius: 3px;
}

.comment-form button {
    background-color: #007bff;
    color: #fff;
    border: none;
    cursor: pointer;
}

.comment-form button:hover {
    background-color: #0056b3;
}

footer {
    background-color: #333;
    color: #fff;
    text-align: center;
    padding: 1rem 0;
}
document.addEventListener('DOMContentLoaded', function() {
    // Gérer le formulaire de publication
    const uploadForm = document.getElementById('uploadForm');
    const gallery = document.getElementById('gallery');
    let mangas = JSON.parse(localStorage.getItem('mangas')) || [];

    const renderMangas = () => {
        gallery.innerHTML = '';
        mangas.forEach((manga, index) => {
            const mangaCard = document.createElement('div');
            mangaCard.classList.add('manga-card');

            const img = document.createElement('img');
            img.src = manga.cover;
            mangaCard.appendChild(img);

            const titleElement = document.createElement('h3');
            titleElement.textContent = manga.title;
            mangaCard.appendChild(titleElement);

            const descriptionElement = document.createElement('p');
            descriptionElement.textContent = manga.description;
            mangaCard.appendChild(descriptionElement);

            const editButton = document.createElement('button');
            editButton.textContent = 'Modifier';
            editButton.addEventListener('click', () => editManga(index));
            mangaCard.appendChild(editButton);

            const commentSection = document.createElement('div');
            commentSection.classList.add('comments');
            
            manga.comments.forEach(comment => {
                const commentElement = document.createElement('p');
                commentElement.textContent = comment;
                commentSection.appendChild(commentElement);
            });

            const commentForm = document.createElement('form');
            commentForm.classList.add('comment-form');
            commentForm.innerHTML = `
                <input type="text" placeholder="Ajouter un commentaire" required>
                <button type="submit">Commenter</button>
            `;
            commentForm.addEventListener('submit', (e) => {
                e.preventDefault();
                const comment = commentForm.querySelector('input').value;
                mangas[index].comments.push(comment);
                localStorage.setItem('mangas', JSON.stringify(mangas));
                renderMangas();
            });
            
            commentSection.appendChild(commentForm);
            mangaCard.appendChild(commentSection);

            gallery.appendChild(mangaCard);
        });
    };

    const editManga = (index) => {
        const manga = mangas[index];
        document.getElementById('title').value = manga.title;
        document.getElementById('description').value = manga.description;
        document.getElementById('uploadStatus').textContent = 'Modification en cours...';

        uploadForm.onsubmit = function(e) {
            e.preventDefault();
            manga.title = document.getElementById('title').value;
            manga.description = document.getElementById('description').value;
            localStorage.setItem('mangas', JSON.stringify(mangas));
            renderMangas();
            document.getElementById('uploadStatus').textContent = 'Manga modifié avec succès !';
            uploadForm.onsubmit = submitManga;
        };
    };

    const submitManga = function(e) {
        e.preventDefault();

        const title = document.getElementById('title').value;
        const description = document.getElementById('description').value;
        const cover = document.getElementById('cover').files[0];
        const file = document.getElementById('file').files[0];

        if (cover && file) {
            const reader = new FileReader();
            reader.onload = function(event) {
                const coverDataURL = event.target.result;

                mangas.push({
                    title,
                    description,
                    cover: coverDataURL,
                    comments: []
                });
[43dcd9a7-70db-4a1f-b0ae-981daa162054](https://github.com/js202005082300/Aide-m-moires/tree/c85c562d508c68fa086972462338ed773a25c8ac/JavaScript%2Fcours%2F002_hello_world%2Fnote.md?citationMarker=43dcd9a7-70db-4a1f-b0ae-981daa162054 "1")[43dcd9a7-70db-4a1f-b0ae-981daa162054](https://github.com/aminechourou/teamleap/tree/6dd62aa770710ac5b28a1b603f513b2bd08b4502/StrongNutrition2%2Fviews%2Fcozastore_2%2Fcozastore%2Fproduct-detail1.php?citationMarker=43dcd9a7-70db-4a1f-b0ae-981daa162054 "2")