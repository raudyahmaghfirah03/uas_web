<template>
  <div class="login-page bg-light">
    <div class="container">
      <div class="row">
        <div class="co-lg-10">
          <div class="bg-white shadow rounded">
            <div class="row">
              <div class="col-md-7 pe-0">
                <div class="form-left h-100 py-5 px-5">
                  <form action="" class="row g-4">
                    <div class="col-12">
                      <label for="">Username <span class="text-danger">*</span></label>
                      <div class="input-group">
                        <div class="input-group-text"><i><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512"><!--! Font Awesome Pro 6.1.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. --><path d="M224 256c70.7 0 128-57.31 128-128s-57.3-128-128-128C153.3 0 96 57.31 96 128S153.3 256 224 256zM274.7 304H173.3C77.61 304 0 381.6 0 477.3c0 19.14 15.52 34.67 34.66 34.67h378.7C432.5 512 448 496.5 448 477.3C448 381.6 370.4 304 274.7 304z"/></svg></i></div>
                        <input type="text" class="form-control" placeholder="Username">
                      </div>
                    </div>
                    <div class="col-12">
                      <label for="">Password <span class="text-danger">*</span></label>
                      <div class="input-group">
                        <div class="input-group-text"><i><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512"><!--! Font Awesome Pro 6.1.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. --><path d="M80 192V144C80 64.47 144.5 0 224 0C303.5 0 368 64.47 368 144V192H384C419.3 192 448 220.7 448 256V448C448 483.3 419.3 512 384 512H64C28.65 512 0 483.3 0 448V256C0 220.7 28.65 192 64 192H80zM144 192H304V144C304 99.82 268.2 64 224 64C179.8 64 144 99.82 144 144V192z"/></svg></i></div>
                        <input type="password" class="form-control" placeholder="Password">
                      </div>
                    </div>

                    <div class="col-sm-6">
                      <div class="form-check">
                        <input type="text" class="form-check-input" id="inlineFormCheck">
                        <label for="inlineFormCheck" class="form-check-labe">Remember</label>
                      </div>
                    </div>
                    <div class="col-sm-6">
                      <a href="#" class="float-end text-primary">Forgot Password</a>
                    </div>
                    <div class="col-12">
                      <button type="submit" class="btn btn-primary px-4 float-end mt-4">Login</button>
                    </div>
                  </form>
                </div>
              </div>
              <div class="col-md-5 ps-0 d-none d-md-block">
                <div class="form-right h-100 background-login text-white-center pt-5">
                  <h2 class="fs-1 text-center">Login User</h2>
                </div>

              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <router-view/>
</template>

<script>
export default {
  name: "LoginView"
}
</script>

<style scoped>
.background-login {
  background-image: url('https://images.unsplash.com/photo-1606107557195-0e29a4b5b4aa?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=764&q=80');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}
.login-page{
  width: 100%;
  height: 100vh;
  display: inline-block;
  display: flex;
  align-items: center;
}
.form-right i {
  font-size: 100px;
}
</style>