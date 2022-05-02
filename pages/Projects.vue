<template>
  <div>
    <div class="projects-block">
      <div class="projects-block-content" v-for="project in projectsList" :key="project.id">
        <div class="project-user">
          <div class="project-user-img">
            <img :src="project.logo_url">
          </div>
          <div class="project-user-name">
            <p @click="openModal(project)">{{ project.name }}</p>
          </div>
        </div>
        <div class="project-condition">
          <p v-if="project.is_active == 1" class="isActive">Active</p>
          <p v-else class="isNotActive">No active</p>
        </div>
        <div class="project-time">
          <div>
            <table>
              <tbody>
                <tr>
                  <td>time this week</td> 
                </tr>
                <tr>
                  <td>this month</td> 
                </tr>
                <tr>
                  <td>total</td> 
                </tr>
              </tbody>
            </table>
            <table>
              <tbody>
                <tr>
                  <td><b>00:00:00</b></td>
                </tr>
                <tr>
                  <td><b>00:00:00</b></td>
                </tr>
                <tr>
                  <td><b>00:00:00</b></td> 
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
    <EditModal 
      v-if="showModal"
      v-model="editingName"
      @close="showModal = false"
      @submit="editProject()"
    />
  </div>
</template>

<script>
import EditModal from "../components/EditModal.vue"

export default {
  name: 'Projects',
  components: {
    EditModal,
  },
  data() {
    return {
      projectsList: [],
      editingProjectId: null,
      editingName: "",
      showModal: false
    }
  },
  mounted() {
    this.getProjects();
  },
  methods: {
    async getProjects() {
      try {
        const token = localStorage.getItem('token');
        const auth = await this.$axios.$get('/projects-manage/index',{
          headers: {
            'Authorization': `Bearer ${token}`,
          }
        });
        this.projectsList = auth.projects;
      } catch (e) {
        console.error(e);
      }
    },
    openModal(project) {
      this.editingProjectId = project.id
      this.editingName = project.name
      this.showModal = true;
    },
    async editProject() {
      try {
        const token = localStorage.getItem('token');
        const payload = { name: this.editingName };
        await this.$axios.$post(`/projects-manage/update?id=${this.editingProjectId}`, payload, {
          headers: {
            'Authorization': `Bearer ${token}`,
          },
        }).then(() => {
          this.getProjects();
          this.showModal = false;
        })
      } catch (e) {
        console.error(e);
      }
    },
  }
}
</script>

<style lang="scss">
.projects-block {
  max-width: 800px;
  width: 100%;
  height: auto;
  margin: 50px auto;
  padding: 25px;
  border: 2px solid #ccc;
  border-radius: 7px;
  background-color: #fff;
  .projects-block-content {
    display: flex;
    justify-content: space-around;
    .project-user, .project-condition, .project-time  {
      width: 100%;
      height: 100px;
      display: flex;
      align-items: center;
    }
    .project-user {
      justify-content: space-around;
      .project-user-img {
        img {
          width: 70px;
        }
      }
      .project-user-name {
        p {
          font-weight: bold;
          font-size: 18px;
          cursor: pointer;
        }
      }
    }
    .project-condition {
      justify-content: center;
      .isActive {
        color: green;
        font-weight: bold;
      }
      .isNotActive {
        color: red;
        font-weight: bold;
      }
    }
    .project-time {
      justify-content: center;
      div {
        width: 200px;
        display: flex;
      }
    }
    @media (max-width: 425px) {
      flex-direction: column;
    }
  }
}
</style>