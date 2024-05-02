<script lang="ts" setup>
  const profileStore = useProfileStore();
const genderStore = useGenderStore();
const email = ref('');
const fio = ref('');
const birthday = ref('');
const gender = ref(null);
async function loadProfile(){
  await profileStore.fetchProfile(); 
  email.value = profileStore.profile.email;
  fio.value = profileStore.profile.fio;
  birthday.value = profileStore.profile.birthday;
  gender.value = profileStore.profile.gender.id;
}
  loadProfile();
  profileStore.fetchReviews();
  profileStore.fetchScores();
  
  const editProfile = () => {
    profileStore.updateProfile({
      fio: fio.value,
      email: email.value,
      birthday: birthday.value,
      gender_id: gender.value
    });
  };
</script>

<template>
  <div v-if="profileStore.profile">
    <h3>{{ profileStore.profile.fio }}</h3>
    <h3>Count scores: {{ profileStore.profile.ratingCount }}</h3>
    <h3>Count reviews: {{ profileStore.profile.reviewCount }}</h3>
    <div class="row">
    <div class="col">
      <ul class="nav nav-pills mb-3" id="pills-tab" role="tablist">
  <li class="nav-item" role="presentation">
    <button class="nav-link active" id="pills-profile-tab" data-bs-toggle="pill" data-bs-target="#pills-profile" type="button" role="tab" aria-controls="pills-profile" aria-selected="true">My profile</button>
  </li>
  <li class="nav-item" role="presentation">
    <button class="nav-link" id="pills-reviews-tab" data-bs-toggle="pill" data-bs-target="#pills-reviews" type="button" role="tab" aria-controls="pills-reviews" aria-selected="false">My reviews</button>
  </li>
  <li class="nav-item" role="presentation">
    <button class="nav-link" id="pills-scores-tab" data-bs-toggle="pill" data-bs-target="#pills-scores" type="button" role="tab" aria-controls="pills-scores" aria-selected="false">My scores</button>
  </li>
</ul>
<div class="tab-content" id="pills-tabContent">
  <div class="tab-pane fade show active" id="pills-profile" role="tabpanel" aria-labelledby="pills-profile-tab" tabindex="0">
  <p>Email: {{ profileStore.profile.email }}</p>
  <p>Birthday: {{ profileStore.profile.birthday }}</p>
  <p>Gender: {{ profileStore.profile.gender.name }}</p>
  <div>
    <button class="btn btn-warning me-2" data-bs-target="#exampleModalToggle" data-bs-toggle="modal">Edit</button>
  <button class="btn btn-danger">Delete Account</button>
  </div>
  </div>
  <div class="tab-pane fade" id="pills-reviews" role="tabpanel" aria-labelledby="pills-reviews-tab" tabindex="0">
  <div>
    <div class="row" 
    v-for="review in profileStore.reviews"
    :key="review.id">
      <div class="col">
        <div class= "row">
              <div class="col fw-bold">{{review.film.name}}</div>
              <div class="col-md-2 text-end">{{ (review.is_approved) ? 'Approved' : 'Deneid' }} </div>
              <div class="col-md-1">{{review.created_at}}</div>
            </div>
            <div class="row">
              <div class="col">{{review.message}}</div>
              <div class="col-md-1">
              <button class="btn btn-danger">Remove</button>
            </div>
            </div>
            <hr>
          </div>
          </div>
        </div>
</div>
  <div class="tab-pane fade" id="pills-scores" role="tabpanel" aria-labelledby="pills-scores-tab" tabindex="0">
    <div class="row"
    v-for="rating in profileStore.scores"
    :key="rating.id">
      <div class="col">
        <div class= "row">
              <div class="col fw-bold">{{rating.film.name}}</div>
              <div class="col-md-1">{{rating.created_at}}</div>
            </div>
            <div class="row">
              <div class="col">{{ rating.score }}</div>
              <div class="col-md-1">
              <button class="btn btn-danger">Remove</button>
            </div>
            </div>
            <hr>
          </div>
          </div>
  </div>
  
</div>
    </div>
    </div>
  </div>
  
  <div class="modal fade" id="exampleModalToggle" aria-hidden="true" aria-labelledby="exampleModalToggleLabel" tabindex="-1">
  <div class="modal-dialog modal-dialog-centered">
    <div class="modal-content">
      <div class="modal-header">
        <h1 class="modal-title fs-5" id="exampleModalToggleLabel">Modal 1</h1>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        <form>
    <div class="form-floating mt-2">
      <input type="text" v-model="fio" class="form-control" id="fio">
      <label for="fio">Username</label>
    </div>
    <div class="form-floating mt-2">
      <input type="email" v-model="email" class="form-control" id="floatingInput" placeholder="name@example.com">
      <label for="floatingInput">Email address</label>
    </div>
    <div class="form-floating">
      <input type="date" v-model="birthday" class="form-control" id="birthday">
      <label for="fio">Birthday</label>
    </div>
    <div class="form-floating">
    <select class="form select" v-model="gender">
    <option value="null">Select gender...</option>
    <option
    v-for="gender in genderStore.genders"
    :key="gender.id"
    :value="gender.id">{{ gender.name }}</option>
    </select>
    </div>
  </form>
      </div>
      <div class="modal-footer">
        <button @click="editProfile" class="btn btn-primary" data-bs-target="#exampleModalToggle2" data-bs-toggle="modal">Save</button>
      </div>
    </div>
  </div>
</div>
</template>

<style>

</style>