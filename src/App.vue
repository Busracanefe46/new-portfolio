<template>
  <div class="portfolio">
    <!-- Google Fonts -->
    <component :is="'script'" src="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,600;0,800;1,400&family=Inter:wght@400;500;600;700&display=swap" />

    <!-- 1. HERO SECTION -->
    <section class="hero-section">
      <div class="hero-content">
        <!-- İsim ve Subtitle -->
        <h1 class="hero-name">Büşra Can Efe</h1>
        <p class="hero-subtitle">Management Information Systems Student & Developer</p>
        
        <!-- MİKRO İSTATİSTİK ROZETLERİ -->
        <div class="hero-stats">
          <div class="stat-badge">
            <span class="stat-icon">🎓</span>
            <span class="stat-text">3rd Year MIS Student</span>
          </div>
          <div class="stat-badge">
            <span class="stat-icon">💻</span>
            <span class="stat-text">Software & Data</span>
          </div>
          <div class="stat-badge">
            <span class="stat-icon">📍</span>
            <span class="stat-text">Ankara, TR</span>
          </div>
        </div>

        <!-- DURUM ROZETİ (Nokta sadeleştirildi) -->
        <div class="status-badge-container">
          <div class="status-badge">
            <span class="pulse"></span>
            {{ currentStatus }}
          </div>
        </div>

        <div class="hero-actions">
          <a href="#about" class="btn primary-btn">Explore Profile</a>
          <a href="#contact" class="btn secondary-btn">Contact Me</a>
        </div>
      </div>
      <div class="scroll-indicator">↓ Scroll to explore</div>
    </section>

    <!-- 2. MAIN BENTO GRID -->
    <main id="about" class="main-container">
      <div class="bento-grid">
        
        <!-- Personal Details -->
        <div class="bento-card details-card glow-card">
          <div class="card-glow-effect"></div>
          <h2>Personal Details</h2>
          <div class="details-list">
            <div class="detail-item">
              <span class="label">Date of Birth</span>
              <span class="value">January 16, 2004</span>
            </div>
            <div class="detail-item">
              <span class="label">Age</span>
              <span class="value">22 Years Old</span>
            </div>
            <div class="detail-item">
              <span class="label">Location</span>
              <span class="value">Türkiye, Ankara</span>
            </div>
          </div>
        </div>

        <!-- About Box -->
        <div class="bento-card about-card glow-card">
          <div class="card-glow-effect"></div>
          <h2>About Me</h2>
          <p>
            I am a Management Information Systems student passionate about software development, 
            database management, and digital technologies. My focus spans across backend technologies like Java and C#, 
            database design with SQL, and modern web development alongside digital marketing strategies.
          </p>
        </div>

        <!-- Education -->
        <div class="bento-card education-card glow-card">
          <div class="card-glow-effect"></div>
          <h2>Education</h2>
          <div class="timeline-item">
            <span class="year">2023 — Present</span>
            <h4>Ankara Yıldırım Beyazıt University</h4>
            <p class="degree">Management Information Systems — 3rd Year Student</p>
            <p>Focusing on software development, web architecture, data management, and business analytics.</p>
          </div>
        </div>

        <!-- Research Interests & Tech Stack -->
        <div class="bento-card interests-card glow-card">
          <div class="card-glow-effect"></div>
          <div class="section-header-flex">
            <h2>Skills & Tech Stack</h2>
            <div class="filter-tabs">
              <button 
                v-for="cat in categories" 
                :key="cat" 
                @click="selectedCategory = cat"
                :class="['filter-btn', { active: selectedCategory === cat }]">
                {{ cat }}
              </button>
            </div>
          </div>

          <div class="tag-cloud">
            <div 
              v-for="skill in filteredSkills" 
              :key="skill.name" 
              class="tag interactive-tag">
              <img v-if="skill.icon" :src="skill.icon" :alt="skill.name" class="badge-icon" />
              <span v-else class="badge-emoji">{{ skill.emoji }}</span>
              <span>{{ skill.name }}</span>
              <span class="skill-level">{{ skill.level }}</span>
            </div>
          </div>

          <!-- Yeni Yetenek Ekleme Formu -->
          <div class="add-skill-box">
            <input v-model="newSkillName" type="text" placeholder="Add new skill..." class="skill-input" />
            <select v-model="newSkillCategory" class="skill-select">
              <option value="Backend">Backend</option>
              <option value="Frontend">Frontend</option>
              <option value="Database">Database</option>
              <option value="Other">Other</option>
            </select>
            <button @click="addNewSkill" class="add-skill-btn">+ Add</button>
          </div>
        </div>

        <!-- Featured Projects (GitHub API Entegreli) -->
        <div class="bento-card projects-card glow-card">
          <div class="card-glow-effect"></div>
          <h2>GitHub Projects</h2>
          
          <!-- Yükleniyor Durumu -->
          <div v-if="loadingProjects" class="loading-text">Loading repositories from GitHub...</div>

          <!-- Proje Listesi -->
          <div v-else class="project-list">
            <a 
              v-for="repo in githubProjects" 
              :key="repo.id" 
              :href="repo.html_url" 
              target="_blank" 
              class="project-item">
              <div class="project-header">
                <h4>{{ repo.name }}</h4>
                <span class="project-link-icon">↗</span>
              </div>
              <p>{{ repo.description || 'No description provided.' }}</p>
              <div class="project-techs">
                <span v-if="repo.language">{{ repo.language }}</span>
                <span>⭐ {{ repo.stargazers_count }}</span>
              </div>
            </a>
          </div>
        </div>

      </div>
    </main>

    <!-- FOOTER / CONTACT -->
    <footer id="contact" class="footer">
      <p>© 2026 Büşra Can — Academic Portfolio</p>
      <div class="footer-links">
        <a href="mailto:busracefe@gmail.com" target="_blank">Email</a>
        <a href="https://github.com/Busracanefe46" target="_blank">GitHub</a>
        <a href="https://www.linkedin.com/in/b%C3%BC%C5%9Fra-can-efe-186488349" target="_blank">LinkedIn</a>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'

const currentStatus = ref('Building my digital portfolio')

const categories = ['All', 'Database', 'Backend', 'Frontend', 'Other']
const selectedCategory = ref('All')

const skillsList = ref([
  { name: 'SQL & Databases', category: 'Database', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/mysql/mysql-original.svg', level: 'Intermediate' },
  { name: 'Java', category: 'Backend', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/java/java-original.svg', level: 'Learning' },
  { name: 'C# Development', category: 'Backend', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/csharp/csharp-original.svg', level: 'Learning' },
  { name: 'Vue.js / Frontend', category: 'Frontend', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/vuejs/vuejs-original.svg', level: 'Intermediate' },
  { name: 'HTML5 & CSS3', category: 'Frontend', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/html5/html5-original.svg', level: 'Intermediate' },
  { name: 'Digital Marketing & E-Commerce', category: 'Other', emoji: '🎯', level: 'Exploring' }
])

const newSkillName = ref('')
const newSkillCategory = ref('Frontend')

const filteredSkills = computed(() => {
  if (selectedCategory.value === 'All') return skillsList.value
  return skillsList.value.filter(s => s.category === selectedCategory.value)
})

const addNewSkill = () => {
  if (!newSkillName.value.trim()) return
  skillsList.value.push({
    name: newSkillName.value.trim(),
    category: newSkillCategory.value,
    emoji: '✨',
    level: 'New'
  })
  newSkillName.value = ''
}

// GitHub API ile Projeleri Çekme Mantığı
const githubProjects = ref([])
const loadingProjects = ref(true)

onMounted(async () => {
  try {
    const response = await fetch('https://api.github.com/users/Busracanefe46/repos?sort=updated&per_page=4')
    const data = await response.json()
    if (Array.isArray(data)) {
      githubProjects.value = data
    }
  } catch (error) {
    console.error('GitHub projeleri çekilemedi:', error)
  } finally {
    loadingProjects.value = false
  }
})
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,600;0,800;1,400&family=Inter:wght@400;500;600;700&display=swap');

:global(html) {
  scroll-behavior: smooth;
}

:global(body) {
  margin: 0;
  font-family: 'Inter', sans-serif;
  background: radial-gradient(circle at 50% 0%, #e0e7ff 0%, #f8fafc 60%, #f1f5f9 100%);
  color: #1e293b;
  line-height: 1.6;
  min-height: 100vh;
}

/* HERO SECTION */
.hero-section {
  min-height: 65vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  padding: 50px 20px 30px 20px;
  position: relative;
}

.hero-name {
  font-family: 'Playfair Display', serif;
  font-size: 72px;
  font-weight: 800;
  color: #0f172a;
  margin: 0 0 10px 0;
  letter-spacing: -1.5px;
  line-height: 1.1;
}

.hero-subtitle {
  font-size: 18px;
  color: #475569;
  margin-bottom: 24px;
}

/* MİKRO İSTATİSTİK ROZETLERİ */
.hero-stats {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
  justify-content: center;
  margin-bottom: 24px;
}

.stat-badge {
  display: flex;
  align-items: center;
  gap: 8px;
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(10px);
  padding: 6px 16px;
  border-radius: 20px;
  border: 1px solid rgba(226, 232, 240, 0.8);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.03);
  font-size: 13px;
  font-weight: 600;
  color: #334155;
}

/* DURUM ROZETİ & PULSE ANIMASYONU */
.status-badge-container {
  margin-bottom: 24px;
  display: flex;
  justify-content: center;
}

.status-badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background: rgba(239, 246, 255, 0.8);
  color: #1e40af;
  padding: 6px 16px;
  border-radius: 20px;
  font-size: 13px;
  font-weight: 600;
  border: 1px solid rgba(191, 219, 254, 0.6);
  backdrop-filter: blur(4px);
}

.pulse {
  width: 8px;
  height: 8px;
  background: #3b82f6;
  border-radius: 50%;
  box-shadow: 0 0 0 0 rgba(59, 130, 246, 0.7);
  animation: pulse-animation 2s infinite;
}

@keyframes pulse-animation {
  0% { transform: scale(0.95); box-shadow: 0 0 0 0 rgba(59, 130, 246, 0.7); }
  70% { transform: scale(1); box-shadow: 0 0 0 6px rgba(59, 130, 246, 0); }
  100% { transform: scale(0.95); box-shadow: 0 0 0 0 rgba(59, 130, 246, 0); }
}

.hero-actions {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
  justify-content: center;
}

.btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 12px 28px;
  border-radius: 10px;
  text-decoration: none;
  font-weight: 600;
  font-size: 15px;
  transition: all 0.3s ease;
}

.primary-btn {
  background: #2563eb;
  color: white;
  box-shadow: 0 4px 14px rgba(37, 99, 235, 0.3);
}

.primary-btn:hover {
  background: #1d4ed8;
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(37, 99, 235, 0.4);
}

.secondary-btn {
  background: rgba(255, 255, 255, 0.9);
  color: #334155;
  border: 1px solid #cbd5e1;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.04);
}

.secondary-btn:hover {
  background: #f8fafc;
  transform: translateY(-2px);
}

.scroll-indicator {
  margin-top: 40px;
  color: #94a3b8;
  font-size: 13px;
}

/* MAIN CONTAINER & BENTO */
.main-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px 20px 60px 20px;
}

.bento-grid {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

/* GLOW & GLASSMORPHISM BENTO CARDS */
.bento-card {
  width: 100%;
  box-sizing: border-box;
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(12px);
  border: 1px solid rgba(226, 232, 240, 0.8);
  border-radius: 18px;
  padding: 30px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.03);
  position: relative;
  overflow: hidden;
  transition: transform 0.3s ease, box-shadow 0.3s ease, border-color 0.3s ease;
}

.card-glow-effect {
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: radial-gradient(circle, rgba(37, 99, 235, 0.08) 0%, rgba(147, 51, 234, 0.03) 40%, transparent 70%);
  opacity: 0;
  transition: opacity 0.4s ease;
  pointer-events: none;
  z-index: 0;
}

.bento-card > * {
  position: relative;
  z-index: 1;
}

.bento-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 20px 40px rgba(37, 99, 235, 0.12);
  border-color: rgba(37, 99, 235, 0.3);
}

.bento-card:hover .card-glow-effect {
  opacity: 1;
}

.bento-card h2 {
  font-size: 19px;
  color: #0f172a;
  margin-top: 0;
  margin-bottom: 18px;
}

.details-card {
  background: rgba(250, 251, 252, 0.85);
}

.details-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.detail-item {
  display: flex;
  flex-direction: column;
}

.detail-item .label {
  font-size: 12px;
  color: #64748b;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.detail-item .value {
  font-size: 15px;
  font-weight: 600;
  color: #0f172a;
}

/* SKILLS & TECH STACK */
.section-header-flex {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 10px;
  margin-bottom: 18px;
}

.section-header-flex h2 {
  margin: 0;
}

.filter-tabs {
  display: flex;
  gap: 6px;
  flex-wrap: wrap;
}

.filter-btn {
  background: rgba(241, 245, 249, 0.8);
  border: 1px solid #e2e8f0;
  padding: 4px 10px;
  border-radius: 6px;
  font-size: 12px;
  font-weight: 500;
  color: #475569;
  cursor: pointer;
  transition: all 0.2s ease;
}

.filter-btn.active, .filter-btn:hover {
  background: #2563eb;
  color: white;
  border-color: #2563eb;
}

.tag-cloud {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-bottom: 18px;
}

.tag {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background: rgba(241, 245, 249, 0.9);
  color: #334155;
  padding: 9px 16px;
  border-radius: 10px;
  font-size: 14px;
  font-weight: 500;
  border: 1px solid #e2e8f0;
  transition: all 0.2s ease;
}

.interactive-tag:hover {
  background: #ffffff;
  transform: translateY(-1px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
}

.badge-icon {
  width: 18px;
  height: 18px;
}

.badge-emoji {
  font-size: 16px;
}

.skill-level {
  margin-left: auto;
  font-size: 11px;
  background: #e2e8f0;
  color: #64748b;
  padding: 2px 6px;
  border-radius: 4px;
}

.add-skill-box {
  display: flex;
  gap: 8px;
  margin-top: 15px;
  padding-top: 15px;
  border-top: 1px solid rgba(226, 232, 240, 0.8);
}

.skill-input, .skill-select {
  padding: 8px 12px;
  border: 1px solid #cbd5e1;
  border-radius: 8px;
  font-size: 13px;
  outline: none;
  background: rgba(248, 250, 252, 0.8);
}

.skill-input {
  flex: 1;
}

.add-skill-btn {
  background: #2563eb;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 8px;
  font-weight: 600;
  font-size: 13px;
  cursor: pointer;
  transition: background 0.2s ease;
}

.add-skill-btn:hover {
  background: #1d4ed8;
}

/* PROJELER (GİTHUB API) */
.loading-text {
  font-size: 14px;
  color: #64748b;
  text-align: center;
  padding: 20px 0;
}

.project-list {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.project-item {
  display: block;
  text-decoration: none;
  background: rgba(248, 250, 252, 0.9);
  padding: 22px;
  border-radius: 14px;
  border: 1px solid #e2e8f0;
  border-left: 4px solid #2563eb;
  transition: all 0.25s ease;
  position: relative;
  overflow: hidden;
}

.project-item:hover {
  background: #ffffff;
  transform: translateX(4px);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.06);
}

.project-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.project-header h4 {
  margin: 0;
  color: #0f172a;
  font-size: 16px;
}

.project-link-icon {
  color: #2563eb;
  font-weight: bold;
}

.project-item p {
  margin: 8px 0 12px 0;
  font-size: 14px;
  color: #64748b;
}

.project-techs {
  display: flex;
  gap: 8px;
}

.project-techs span {
  font-size: 12px;
  background: #e2e8f0;
  color: #475569;
  padding: 3px 10px;
  border-radius: 6px;
  font-weight: 500;
}

/* TIMELINE */
.timeline-item .year {
  font-size: 12px;
  color: #2563eb;
  font-weight: 700;
}

.timeline-item h4 {
  margin: 6px 0 2px 0;
  color: #0f172a;
  font-size: 17px;
}

.timeline-item .degree {
  color: #2563eb;
  font-weight: 600;
  font-size: 14px;
  margin: 0 0 6px 0;
}

/* FOOTER */
.footer {
  text-align: center;
  padding: 40px 20px;
  border-top: 1px solid rgba(226, 232, 240, 0.8);
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(10px);
}

.footer-links {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-top: 10px;
}

.footer-links a {
  color: #2563eb;
  text-decoration: none;
  font-size: 14px;
  font-weight: 500;
}

@media (max-width: 600px) {
  .hero-name {
    font-size: 48px;
  }
}
</style>