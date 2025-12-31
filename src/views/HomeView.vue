<script setup>
import info from '../services/main_information.json'
import works from '../services/work_experience.json'
import publications from '../services/publications.json'
import research from '../services/research.json'
import projects from '../services/projects.json'
import organizations from '../services/organizations.json'

const year = new Date().getFullYear()
</script>

<template>
  <main id="top">
    <header class="divide">
      <div>
        <h1>{{ info.name }}</h1>
        <div>
          <p>{{ info.address }}</p>
          <a :href="'mailto:' + info.email">{{ info.email }}</a>
        </div>
      </div>

      <div>
        <a :href="info.github" target="_blank" rel="noopener noreferrer">
          <img src="/icon/github.svg" alt="GitHub">
        </a>

        <a :href="info.linkedin" target="_blank" rel="noopener noreferrer">
          <img src="/icon/linkedin.svg" alt="LinkedIn">
        </a>
      </div>
    </header>

    <hr>

    <section>
      <h2 class="green bottom-1">{{ info.roles }}</h2>

      <p>{{ info.about_me }}</p>

      <p><strong>Tags: </strong>{{ info.keywords.join(', ') }}</p>
    </section>

    <hr>

    <section>
      <h3 class="green bottom-1">Projects</h3>

      <div class="projects">
        <div v-for="(proj, pIndex) in projects" :key="pIndex" class="project-item">
          <div class="project-meta">
            <h2 class="project-title">{{ proj.title }}</h2>

            <p class="project-desc">{{ proj.description }}</p>

            <div v-if="proj.repo" class="center">
              <img src="/icon/github.svg" alt="GitHub" class="icon">
              <a :href="proj.repo" target="_blank" rel="noopener noreferrer">{{ proj.repo }}</a>
            </div>
          </div>
        </div>
      </div>
    </section>

    <hr>

    <section>
      <h3 class="green bottom-1">Work Experience</h3>

      <div class="section-wrap">
        <div v-for="(work, wIndex) in works" :key="wIndex" class="section">
          <div class="section-header">
            <h2>{{ work.company }}</h2>
            <a :href="work.company_url" target="_blank" rel="noopener noreferrer">{{ work.company_url }}</a>
            <h3>{{ work.roles }}</h3>
            <p>{{ work.date }}</p>
          </div>

          <div class="section-detail">
            <div>
              <div v-for="(job, jIndex) in work.jobs" :key="jIndex" class="job-block">
                <h4>{{ job.title }}</h4>

                <div v-if="Array.isArray(job.items)">
                  <div v-for="(item, itemIndex) in job.items" :key="itemIndex" class="job-item">
                    <p v-if="item.title" class="job-item-title">{{ item.title }}</p>

                    <ul v-if="Array.isArray(item.points) && item.points.length">
                      <li v-for="(point, pIndex) in item.points" :key="pIndex" class="job-point">
                        <template v-if="point && typeof point === 'string'">
                          <div v-html="point"></div>
                        </template>
                        <template v-else>
                          <div v-if="point.point_name" v-html="point.point_name"></div>

                          <ul v-if="Array.isArray(point.points) && point.points.length" class="job-subpoints">
                            <li v-for="(sub, sIndex) in point.points" :key="sIndex">
                              <div v-if="typeof sub === 'string'" v-html="sub"></div>
                              <div v-else v-html="sub"></div>
                            </li>
                          </ul>
                        </template>
                      </li>
                    </ul>
                  </div>
                </div>

                <!-- Fallback if job.items is an object (backwards compatibility) -->
                <div v-else-if="job.items && job.items.title">
                  <p class="job-item-title">{{ job.items.title }}</p>
                  <ul v-if="job.items.points && job.items.points.length">
                    <li v-for="(point, pIndex) in job.items.points" :key="pIndex">
                      <template v-if="point && typeof point === 'string'">
                        <div v-html="point"></div>
                      </template>
                      <template v-else>
                        <div v-if="point.point_name" v-html="point.point_name"></div>
                        <ul v-if="point.points && point.points.length">
                          <li v-for="(sub, sIndex) in point.points" :key="sIndex">
                            <div v-if="typeof sub === 'string'" v-html="sub"></div>
                            <div v-else v-html="sub"></div>
                          </li>
                        </ul>
                      </template>
                    </li>
                  </ul>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <hr>

    <section>
      <h3 class="green bottom-1">Research</h3>

      <div class="section-wrap">
        <div v-for="(r, idx) in research" :key="idx" class="section">
          <div class="section-header">
            <h3>{{ r.role }}</h3>
            <p v-if="r.assistant_for"><strong>Assistant for</strong> {{ r.assistant_for }}</p>
            <a :href="r.link" v-if="r.link">{{ r.link }}</a>
            <span v-if="r.promotor"><strong>Promotor:</strong> {{ r.promotor }}</span>
            <p>{{ r.institution }}</p>
            <p>{{ r.period }}</p>
            <!-- <a :href="r.link" target="_blank" rel="noopener noreferrer">{{ r.link }}</a> -->
          </div>

          <div class="section-detail">
            <h3 class="research-project">{{ r.project }}</h3>
            <p>{{ r.description }}</p>
            <div v-if="r.responsibilities && r.responsibilities.length" class="research-responsibilities">
              <strong>Responsibilities:</strong>
              <ul>
                <li v-for="(resp, ridx) in r.responsibilities" :key="ridx">{{ resp }}</li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </section>

    <hr>

    <section>
      <h3 class="green bottom-1">Publications</h3>

      <div>
        <div v-for="(pub, i) in publications" :key="i" class="publication">
          <div class="pub-meta">
            <h3>{{ pub.title }}</h3>
            <p class="pub-authors">{{ pub.authors.join(', ') }}</p>
            <p class="pub-details">
              {{ pub.journal }}
              <span v-if="pub.volume">, vol. {{ pub.volume }}</span>
              <span v-if="pub.issue"> ({{ pub.issue }})</span>
              <span v-if="pub.pages">, pp. {{ pub.pages }}</span>
              , {{ pub.year }}
            </p>
            <p v-if="pub.publisher" class="pub-publisher">Publisher: {{ pub.publisher }}</p>
            <p v-if="pub.abstract" class="pub-abstract">{{ pub.abstract }}</p>

            <p class="pub-links">
              <span v-if="pub.doi"> <a :href="'https://doi.org/' + pub.doi" target="_blank"
                  rel="noopener noreferrer">DOI</a></span>
              <span v-if="pub.link"> <a :href="pub.link" target="_blank" rel="noopener noreferrer">View</a></span>
            </p>
          </div>

          <div class="pub-qr">
            <a :href="pub.link || ('https://doi.org/' + pub.doi)" target="_blank" rel="noopener noreferrer">
              <img
                :src="`https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=${encodeURIComponent(pub.link || ('https://doi.org/' + pub.doi))}`"
                :alt="`QR code for ${pub.title}`" />
            </a>
          </div>
        </div>
      </div>
    </section>

    <hr>

    <section>
      <h3 class="green bottom-1">Organization</h3>

      <div class="section-wrap">
        <div v-for="(org, oIdx) in organizations" :key="oIdx" class="section">
          <div class="section-header">
            <h3>{{ org.organization }}</h3>
            <p><strong>{{ org.role }}</strong></p>
            <a :href="org.link" target="_blank" rel="noopener noreferrer">{{ org.link }}</a>
            <p>{{ org.period }}</p>
          </div>

          <div class="section-detail">
            <p>{{ org.description }}</p>
          </div>
        </div>
      </div>
    </section>

    <hr>

    <section>
      <h3 class="green bottom-1">Education</h3>

      <div>
        <h3>Bachelor of Computer Science</h3>
        <p>Gunadarma University, 2022 - 2026</p>
      </div>
    </section>

    <footer class="footer">
      <div class="footer-inner">
        <div class="footer-left">
          <p><strong>{{ info.name }}</strong> — <a :href="'mailto:' + info.email">{{ info.email }}</a></p>
          <p>{{ info.address }}</p>
        </div>

        <div class="footer-center">
          <a :href="info.github" target="_blank" rel="noopener noreferrer"><img src="/icon/github.svg" alt="GitHub"
              class="icon"></a>
          <a :href="info.linkedin" target="_blank" rel="noopener noreferrer"><img src="/icon/linkedin.svg"
              alt="LinkedIn" class="icon"></a>
        </div>

        <div class="footer-right">
          <p>© {{ year }} {{ info.name }}</p>
          <p><a href="#top">Back to top</a></p>
        </div>
      </div>
    </footer>
  </main>
</template>

<style scoped>
  ::v-deep em {
  background-color: #444444;
  color: white;
  padding: 0 0.3rem;
}
</style>