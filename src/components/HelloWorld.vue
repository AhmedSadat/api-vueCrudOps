<template>
  <div class="rootDiv">
    <h1>Welcome to our Members Website</h1>
    <p>this is website for 700Apps Pega Team</p>
    <div class="members">
      <button @click="show = !show" class="btn btn-primary">Click To Show Members</button>
      <table class="table" v-if="show">
        <thead class="thead-dark">
          <tr>
            <th scope="col">#</th>
            <th scope="col">Name</th>
            <th scope="col">Job</th>
            <th scope="col">To Update</th>
            <th scope="col">To Delete</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="  (member , index) in members" :key="index">
            <th scope="row">{{index + 1}}</th>
            <td>{{member.name}}</td>
            <td>{{member.job}}</td>
            <td>
              <button class="btn btn-secondary" @click="populateUpdateMemberModel(member)">Update</button>
            </td>
            <td>
              <button class="btn btn-danger" @click="deleteMember(member._id)">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="container">
      <div class="CreateForm">
        <h3>Create Form</h3>
 
        <form>
          <div class="row">
            <div class="col-25">
              <label for="fname">Name</label>
            </div>
            <div class="col-75">
              <input
                type="text"
              
                v-model="member.name"
                id="fname"
                name="firstname"
                placeholder="Your name.."
              />
           
            </div>
          </div>
          <div class="row">
            <div class="col-25">
              <label for="lname">Job</label>
            </div>
            <div class="col-75">
              <input
                type="text"
                required
                v-model="member.job"
                id="lname"
                name="lastname"
                placeholder="Your job.."
              />
              
      
            </div>
          </div>
          <div class="row">
            <button
          
              @click.prevent="createMember()"
            >Create Member</button>
          </div>
        </form>
     
      </div>
      <div class="UpdateForm">
        <h3>UpdateForm</h3>
        <form>
          <div class="row">
            <div class="col-25">
              <label for="fname">Name</label>
            </div>
            <div class="col-75">
              <input
                type="text"
                required
                v-model="updatedMember.name"
                id="fname"
                name="firstname"
                placeholder="Your name.."
              />
            </div>
          </div>
          <div class="row">
            <div class="col-25">
              <label for="lname">Job</label>
            </div>
            <div class="col-75">
              <input
                type="text"
                required
                v-model="updatedMember.job"
                id="lname"
                name="lastname"
                placeholder="Your job.."
              />
            </div>
          </div>
          <div class="row">
            <button
              
              @click.prevent="updateMember"
            >Update Member</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";




export default {
  created: function() {
     this.getData();
  },
  data: () => {
    return {
      members: [],
      show: false,
      member: {
        name: "",
        job: ""
      },
      updatedMember: {
        name: "",
        job: ""
      },
      memberID:''
    };
  },
  methods: {
    async createMember() {
      const result = await axios.post(
        "http://localhost:4000/api/members",
        this.member
      );
      console.log(result);
      this.members.push(result.data);
      this.member.name = "";
      this.member.job = "";
      console.log("triggered");
    },

    async getData() {
      try {
        const result = await axios.get("http://localhost:4000/api/members");
        console.log(result.data);
        this.members = result.data;
        this.show = true;
      } catch (ex) {
        console.log(ex);
      }
    },  

    populateUpdateMemberModel(member){
      this.updatedMember.name = member.name;
      this.updatedMember.job = member.job;
      this.memberID = member._id
    },

    async updateMember() {
     
       await axios.put(
        `http://localhost:4000/api/members/${this.memberID}`,
        this.updatedMember
      );
      this.updatedMember.name = "" ;
      this.updatedMember.job  = ""
      
      this.getData();
     
    },

    async deleteMember(id) {
      const result = await axios.delete(
        `http://localhost:4000/api/members/${id}`
      );
      const index = this.members.findIndex(el => {
        return el._id === result.data._id;
      });

      const deletedMember = this.members.splice(index, 1);
      console.log(deletedMember);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/* Style inputs, select elements and textareas */
input[type="text"],
select,
textarea {
  width: 80%;
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
  resize: vertical;
  margin: 5px auto;
}

/* Style the label to  next to the inputs */
label {
  padding: 12px 12px 12px 0;
  display: inline-block;
}

/* Style the submit button */
input[type="submit"] {
  background-color: #4caf50;
  color: white;
  padding: 12px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin: auto;
}

.UpdateForm button {
  margin: 10px auto;
  border-radius: 6px;
  color: white;
  outline: none;
  background: darkblue;
  padding: 10px;
}

.CreateForm button {
  margin: 10px auto;
  border-radius: 6px;
  color: white;
  outline: none;
  background: darkgreen;
  padding: 10px;
}

/* Style the container */

/* Floating column for labels: 25% width */
.col-25 {
  float: left;
  width: 25%;
  margin-top: 6px;
}

/* Floating column for inputs: 75% width */
.col-75 {
  float: left;
  width: 75%;
  margin-top: 6px;
}

/* Clear floats after the columns */
.row:after {
  content: "";
  display: table;
  clear: both;
}

/* Responsive layout - when the screen is less than 600px wide, make the two columns stack on top of each other instead of next to each other */
@media screen and (max-width: 600px) {
  .col-25,
  .col-75,
  input[type="submit"] {
    width: 100%;
    margin-top: 0;
  }
}

.rootDiv {
  text-align: center;
}

.members button {
  padding: 5px 5px;
  margin: 5px 0px;
}
.members table {
  width: 70%;
  margin: 5px auto;
}

.container {
  background-color: #fdfdfd;
  height: 70%;
  width: 70%;
  border-radius: 6px;
  box-shadow: 0 4px 28px rgba(123, 151, 158, 0.25);
  border: 1px solid #d6dee1;
  padding: 2rem;
  margin: 5px auto;
  box-sizing: border-box;
}

form {
  margin-bottom: 7px;
}
</style>
