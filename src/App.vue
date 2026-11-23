
<script setup>
  import { ref } from 'vue'

  const activeTab = ref('Profile')
  const selectedProject = ref(null)

  const tabs = [
    'Profile',
    'Projects',
    'Art / 3D',
    'Progress Log'
  ]

  const projects = [
    {
      name: 'Headless Hotel',
      type: 'Game Development',
      description: 'Worked on enemy design and behaviors',
      date: '2024',
      mediaType: 'youtube',
      url: 'https://www.youtube.com/watch?v=ul1oyhH_fj8'
    },
    {
      name: 'Dialogue System',
      type: 'Programming',
      description: 'Very simplistic visual novel implementation in Unity.',
      date: '2025',
      mediaType: 'itch',
      url: 'https://itch.io/embed-upload/19620169?color=333333'
    },
    {
      name: 'Skyfarer',
      type: 'Game Development',
      description: 'Worked on low poly 3D models and postprocessing.',
      date: '2023',
      mediaType: 'youtube',
      url: 'https://www.youtube.com/watch?v=GQN3QScx0tI'
    }
  ]

  const art = [
    {
      name: '3D Modeling Weapons',
      type: 'Maya | Blender',
      mediaType: '3d',
      url: 'https://sketchfab.com/models/127ad477a3bf41b6adca4c773365542b/embed'
    },
    {
      name: 'Character Modeling',
      type: 'Digital Art',
      mediaType: 'image',
      images: [
        '/webimages/example3.png',
        '/webimages/example4.png',
        '/webimages/example5.png',
        '/webimages/example6.png'
      ]
    },
    {
      name: 'Custom Shaders',
      type: 'Digital Art',
      mediaType: 'image',
      images: [
        '/webimages/example.png',
        '/webimages/example2.png'
      ]
    },
    {
      name: '3D Animation',
      type: 'Unity | Maya',
      mediaType: 'youtube',
      url: 'https://www.youtube.com/watch?v=fqMIwuafC7g'
    },
    {
      name: 'Particle Effects', 
      type: 'Unity',
      mediaType: 'video',
      url: '/videos/particles.mp4'
    }
  ]

  const updates = [
    {
      date: '11.22.2026',
      title: 'STRIX',
      description: 'Currently working on personal project'
    }
  ]

  function openProject(project) {
    selectedProject.value = project
  }

  function closeProject() {
    selectedProject.value = null
  }

  // Convert different YouTube URL formats into embed URLs
  function getYouTubeEmbed(url) {
    try {
      const parsed = new URL(url)
      let videoId = ''

      if (parsed.hostname.includes('youtu.be')) {
        videoId = parsed.pathname.slice(1).split('/')[0]
      } else if (parsed.pathname === '/watch') {
        videoId = parsed.searchParams.get('v')
      } else {
        const match = parsed.pathname.match(
          /\/(?:embed|shorts|live)\/([^/?]+)/
        )
        videoId = match ? match[1] : ''
      }

      if (!videoId) return ''

      return `https://www.youtube-nocookie.com/embed/${videoId}?rel=0`
    } catch {
      return ''
    }
  }

  // Get the correct URL for each media type
  function getProjectEmbed(project) {
    if (project.mediaType === 'youtube') {
      return getYouTubeEmbed(project.url)
    }

    return project.url
  }
</script>

<template>
  <main class="container">

    <header>
      <a class="brand" @click="activeTab = 'Profile'">
        Magllama.moe
      </a>
    </header>

    <nav>
      <button v-for="(tab, index) in tabs"
              :key="tab"
              :class="{ active: activeTab === tab }"
              @click="activeTab = tab">
        <span class="tab-number">0{{ index + 1 }}</span>
        {{ tab }}
      </button>
    </nav>

    <section class="content">

      <!-- PROFILE -->
      <div v-if="activeTab === 'Profile'">

        <h1>Hi<span>.</span></h1>

        <p class="intro">
          I like playing card games, mobas, and roguelikes.
          I am also addicted to gachas.
        </p>

        <div class="divider"></div>

        <h3>ABOUT ME</h3>
        <p class="body-text">
          I am a student at New York University majoring in Game Design,
          and minoring in Computer Science and Business.
        </p>

        <div class="info-grid">
          <div>
            <span>Proficiency</span>
            <p>Unity / Maya / Blender / C#</p>
          </div>
        </div>

        <div class="divider"></div>

        <h3>CONTACT</h3>
        <div class="contact-links">
          <a href="mailto:ansinfuego1@gmail.com">Email ↗</a>
          <a href="https://github.com/" target="_blank">GitHub ↗</a>
          <a href="https://discord.com/users/magllama" target="_blank">Discord ↗</a>
        </div>

      </div>

      <!-- PROJECTS -->
      <div v-else-if="activeTab === 'Projects'">

        <h1>(Kinda) Complete Projects<span>.</span></h1>

        <div class="divider"></div>

        <article v-for="project in projects"
                 :key="project.name"
                 class="list-item clickable"
                 @click="openProject(project)">
          <div>
            <span class="category">{{ project.type }}</span>
            <h3>{{ project.name }} <span class="project-arrow">↗</span></h3>
            <p class="body-text">{{ project.description }}</p>
          </div>

          <span class="date">{{ project.date }}</span>
        </article>

      </div>

      <!-- ART / 3D -->
      <div v-else-if="activeTab === 'Art / 3D'">

        <h1>Art Stuff<span>.</span></h1>

        <div class="divider"></div>

        <div class="art-grid">
          <div v-for="item in art"
               :key="item.name"
               class="art-item clickable"
               @click="openProject(item)">

            <div class="art-placeholder">
              <img v-if="item.images?.length"
                   :src="item.images[0]"
                   :alt="item.name" />
              <span v-else>VIEW / ↗</span>
            </div>

            <h3>{{ item.name }} <span class="project-arrow">↗</span></h3>
            <span class="category">{{ item.type }}</span>

          </div>
        </div>

      </div>

      <!-- PROGRESS LOG -->
      <div v-else-if="activeTab === 'Progress Log'">

        <h1>Things I'm doing right now<span>.</span></h1>


        <div class="divider"></div>

        <article v-for="update in updates"
                 :key="update.title"
                 class="log-item">
          <span class="date">{{ update.date }}</span>

          <div>
            <h3>{{ update.title }}</h3>
            <p class="body-text">{{ update.description }}</p>
          </div>
        </article>

      </div>

    </section>

    <!-- PROJECT MODAL -->
    <div v-if="selectedProject"
         class="modal-overlay"
         @click.self="closeProject">
      <div class="modal">

        <button class="modal-close"
                @click="closeProject"
                aria-label="Close project">
          ✕
        </button>

        <span class="category">
          {{ selectedProject.type }}
        </span>

        <h2 class="modal-title">
          {{ selectedProject.name }}
        </h2>

        <p class="body-text">
          {{ selectedProject.description }}
        </p>

        <!-- MEDIA DISPLAY -->
        <div class="media-container"
             :class="{
            'itch-container': selectedProject.mediaType === 'itch'
          }">

          <!-- YOUTUBE -->
          <iframe v-if="selectedProject.mediaType === 'youtube'"
                  :src="getProjectEmbed(selectedProject)"
                  :title="selectedProject.name"
                  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
                  allowfullscreen></iframe>

          <!-- LOCAL VIDEO -->
          <video v-else-if="selectedProject.mediaType === 'video'"
                 :src="selectedProject.url"
                 controls
                 autoplay
                 playsinline></video>

          <!-- IMAGE GALLERY -->
          <div v-else-if="selectedProject.images?.length"
               class="image-gallery">

            <img v-for="(image, index) in selectedProject.images"
                 :key="image"
                 :src="image"
                 :alt="`${selectedProject.name} - Image ${index + 1}`" />

          </div>

          <!-- SINGLE IMAGE FALLBACK -->
          <img v-else-if="selectedProject.mediaType === 'image'"
               :src="selectedProject.url"
               :alt="selectedProject.name" />

          <!-- ITCH.IO GAME -->
          <iframe v-else-if="selectedProject.mediaType === 'itch'"
                  :src="selectedProject.url"
                  :title="selectedProject.name"
                  allowfullscreen
                  allow="autoplay; fullscreen; gamepad; accelerometer; gyroscope; clipboard-read; clipboard-write"></iframe>

          <!-- 3D MODEL VIEWER (SKETCHFAB) -->
          <iframe v-else-if="selectedProject.mediaType === '3d'"
                  :src="selectedProject.url"
                  :title="selectedProject.name"
                  allow="autoplay; fullscreen; xr-spatial-tracking"
                  allowfullscreen></iframe>

          <!-- GENERIC WEBSITE EMBED -->
          <iframe v-else-if="selectedProject.mediaType === 'embed'"
                  :src="selectedProject.url"
                  :title="selectedProject.name"
                  allowfullscreen></iframe>

          <!-- EXTERNAL LINK -->
          <div v-else-if="selectedProject.mediaType === 'link'"
               class="external-project">
            <p>View this project externally.</p>
            <a :href="selectedProject.url"
               target="_blank"
               rel="noopener noreferrer">
              Open Project ↗
            </a>
          </div>

        </div>

        <div class="modal-footer">
          <span>{{ selectedProject.date }}</span>
        </div>

      </div>
    </div>

    <footer>
      <span>© 2026 Magllama</span>
      <span>LAST UPDATED: 11.17.2026</span>
    </footer>

  </main>
</template>
