<template>
  <div class="container">
    <div class="about-section">
  <h1>About Us</h1>
  <p>We are Arie and Eden, university students that decided to build this site as part of our work in a course.</p>
  <p>We hope you will enjoy the following background on us and our motivation for this project.</p>
  <p>Our previous works:</p>

  <p>
  <a href="https://github.com/Web-Development-Environments-2022/209406164">Personal website for seniors who are in isolation</a>
  </p>

  <p>
  <a href="https://github.com/Web-Development-Environments-2022/assignment2-206259848_209406164">Pacman Game</a>
  </p>

  <p>
  <a href="https://github.com/Web-Development-Environments-2022/assignment3_2-209406164_206259848">Server side of this recipes website</a>
  </p>

  <p>
  <a href="https://github.com/Web-Development-Environments-2022/assignment3-3-209406164_206259848">Client side of this recipes website</a>
  </p>

  

</div>

<div class="row">
  <div class="column">
    <div class="card">
      <img src="../assets/bgu-1.jpg" alt="BGU" style="width:100%;height:200px">
      <div class="container">
        <h2 style="text-align:center">Ben-Gurion University</h2>
        <p style="text-align:center" class="title">Our institute</p>
        <p>This is the research university that we do our degree at. Its motto is "	Inspiration Meets Excellence" and it greatly inspires us on daily basis.</p>
      </div>
    </div>
  </div>

  <div class="column">
    <div class="card">
      <img src="../assets/build-website.jpg" alt="Course" style="width:100%;height:200px">
      <div class="container">
        <h2 style="text-align:center">Internet development environments</h2>
        <p style="text-align:center" class="title">Our course</p>
        <p></p>
        <p>The course deals with understanding the architecture
          and infrastructure of the Internet while gaining practical
          experience in setting up a web application using design and
          development tools for setting up applications in the Internet environment.</p>
      </div>
    </div>
  </div>

  <div class="column">
    <div class="card">
      <img src="../assets/diy-food-photography-backgrounds.jpg" alt="Food" style="width:100%;height:200px">
      <div class="container">
        <h2 style="text-align:center">Food</h2>
        <p style="text-align:center" class="title">Our passion</p>
        <p>If there is anything we love more than programming and creating, it is making food which is its own form of art. Just like programming it requires creativity and precice work to deliver the best results. </p>
      </div>
    </div>
  </div>
</div>
  </div>
</template>

<style lang="scss" scoped>
body {
  font-family: Arial, Helvetica, sans-serif;
  margin: 0;
}

html {
  box-sizing: border-box;
}

*, *:before, *:after {
  box-sizing: inherit;
}

.column {
  float: left;
  width: 33.3%;
  margin-bottom: 16px;
  padding: 0 8px;
}

.card {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  margin: 8px;
  height: 480px;
}

.about-section {
  padding: 50px;
  text-align: center;
  background-color: #474e5d;
  color: white;
}

.container {
  padding: 0 16px;
}

.container::after, .row::after {
  content: "";
  clear: both;
  display: table;
}

.title {
  color: grey;
}

@media screen and (max-width: 650px) {
  .column {
    width: 100%;
    display: block;
  }
}



</style>