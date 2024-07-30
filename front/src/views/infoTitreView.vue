<template>
  <div class="container">
    <h1>{{ music.title }}</h1>
    <img :src="image" alt="Album Cover" class="album-cover">
    <p>Durée: {{ formatDuration(music.length) }}</p>
    <p>Sortie: {{ music.releaseDate }}</p>
    <h2>Artistes :</h2>
    <ul class="artist-list">
      <li v-for="(artist, index) in music['artist-credit']" :key="index">
        <router-link :to="{ name: 'infoArtiste', params: { id: artist.artist.id } }">
          {{ artist.name }}
        </router-link>
      </li>
    </ul>
    <button @click="$router.go(-1)"><i class="fas fa-arrow-left"></i> Retour</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      music: {
        id: '',
        title: '',
        length: '',
        releaseDate: '',

        'artist-credit': []
      },
      image: ''
    };
  },
  methods: {
    /*
    / méthode qui convertit la durée de la musique en minutes et secondes au lieu de millisecondes
    */
    formatDuration(duration) {
      let seconds = Math.floor(duration / 1000);
      let minutes = Math.floor(seconds / 60);
      seconds = seconds % 60;
      return `${minutes} min ${seconds} sec`;
    }
  },
  created() {
  // Récupération de l'ID et de l'image de la route
  const id = this.$route.params.id;
  this.image = this.$route.params.image;

  // Appel à l'API MusicBrainz pour obtenir les détails de l'enregistrement
  fetch(`https://musicbrainz.org/ws/2/recording/${id}?inc=artist-credits+releases&fmt=json`)
    .then(response => response.json()) // Conversion de la réponse en JSON
    .then(data => {
      // Création de l'objet music avec les détails de l'enregistrement
      this.music = {
        id: data.id,
        title: data.title,
        length: data.length,
        releaseDate: data.releases[0]['title'],
        'artist-credit': data['artist-credit']
      };

      // Si aucune image n'a été passée en paramètre de route, utilisation de l'image de l'enregistrement
      if (!this.image) {
        this.image = this.music.image;
      }
    })
    .catch(error => {
      // Affichage d'une erreur si l'appel à l'API échoue
      console.error('Erreur lors de la récupération des détails de la musique:', error);
    });
}
};
</script>


<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
}

h1 {
  margin-bottom: 20px;
  color: #333;
  font-size: 2em;
}

.album-cover {
  width: 200px;
  height: 200px;
  object-fit: cover;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.artist-list {
  list-style-type: none;
  padding: 0;
}

.artist-list li {
  margin-bottom: 10px;
  font-size: 1.2em;
  color: #666;
}
</style>