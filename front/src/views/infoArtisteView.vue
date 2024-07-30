<template>
  <div class="container">
    <h1>{{ artist.name }}</h1>
    <img src="../assets/profil.jpg" alt="Erreur" class="imageprofil" width="50px" height="50px">
    
    <p>Pays: {{ artist.country }}</p>
    <p> Type: {{ artist.type }}</p>
    <p>{{ artist.lifeSpan }}</p>
    <p>Style: {{ artist.disambiguation }}</p>

    <div class="container">
  <h2>Albums</h2>
  <ul class="album-list">
    <li class="album-item" v-for="(album, index) in albums" :key="index">
      {{ album.title }}
    </li>
  </ul>
</div>
<button @click="$router.go(-1)"><i class="fas fa-arrow-left"></i> Retour</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      artist: {
        id: '',
        name: '',
        country: '',
        type: '',
        lifeSpan: '',
        disambiguation: '',
      },
      albums: []
    };
  },
  
  created() {
    const id = this.$route.params.id;
    fetch(`https://musicbrainz.org/ws/2/artist/${id}?fmt=json`)
        .then(response => response.json())
        .then(data => {
          this.artist = {
            id: data.id,
            name: data.name,
            country: data.country,
            type: data.type,
            lifeSpan: data['life-span'] ? `${data['life-span'].begin} - ${data['life-span'].ended ? data['life-span'].end : 'Present'}` : 'Unknown',
            disambiguation: data.disambiguation,
          };
        })
        .catch(error => {
          console.error('Error fetching artist details:', error);
        });

    fetch(`https://musicbrainz.org/ws/2/release-group?artist=${id}&fmt=json`)
        .then(response => response.json())
        .then(data => {
          this.albums = data['release-groups'].map(rg => ({ title: rg.title }));
        })
        .catch(error => {
          console.error('Error fetching albums:', error);
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

.imageprofil {
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

.album-list {
  list-style-type: none;
  padding: 0;
}

.album-item {
  border: 1px solid #ddd;
  margin-bottom: 10px;
  padding: 10px;
}
</style>

 