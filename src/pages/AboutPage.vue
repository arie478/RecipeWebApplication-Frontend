<template>
  <div class="container">
    <div class="about-section">
      <h1>About Us</h1>
      <p>We are Arie and Eden, university students that decided to build this site as part of our work in a course.</p>
      <p>We hope you will enjoy the following background on us and our motivation for this project.</p>
    </div>

    <div class="row">

      <div class="column">
        <div class="card">
          <img src="../assets/Eden_Site.png" style="width:100%;height:200px">
          <div class="container">
            <h2 style="text-align:center">Eden's Site</h2>
            <p style="text-align:center" class="title">Assignment 1</p>
            <p>In this project I built a personal website about an elderly person. I chose to do my work on a person who I think is very interesting - Ze'ev Revach</p>
            <center>
              <a href="https://web-development-environments-2022.github.io/206259848/">Visit My Website</a>
            </center>
          </div>
        </div>
      </div>

      <div class="column">
        <div class="card">
          <img src="../assets/Arie_Site.png" style="width:100%;height:200px">
          <div class="container">
            <h2 style="text-align:center">Arie's Site</h2>
            <p style="text-align:center" class="title">Assignment 1</p>
            <p>In this exercise I have built a personal website about an adult in old age who is in isolation. This person is András Arató, who is also known as "Hide the pain Harold".</p>
            <center>
              <a href="https://web-development-environments-2022.github.io/209406164/">Visit My Website</a>
            </center>
          </div>
        </div>
      </div>

      <div class="column">
        <div class="card">
          <img src="../assets/pacman_2.png" style="width:100%;height:200px">
          <div class="container">
            <h2 style="text-align:center">Pacman Game</h2>
            <p style="text-align:center" class="title">Assignment 2</p>
            <p>This is a pacman game for our web development course. We used JavaScript, jQuery, HTML and CSS.</p>
            <br>
            <center>
              <a href="https://web-development-environments-2022.github.io/assignment2-206259848_209406164/">Start Playing</a>
            </center>

          </div>
        </div>
      </div>
    </div>

    <div class="row">

      <div class="column">
        <div class="card">
          <img src="../assets/bgu-1.jpg" alt="BGU" style="width:100%;height:200px">
          <div class="container">
            <h2 style="text-align:center">Ben-Gurion University</h2>
            <p style="text-align:center" class="title">Our institute</p>
            <p>This is the research university that we do our degree at. Its motto is " Inspiration Meets Excellence" and it greatly inspires us on daily basis.</p>
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

*,
*:before,
*:after {
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
  background-color: rgb(21, 174, 185);
  color: white;
  margin-top: 10px;
}

.container {
  padding: 0 16px;
}

.container::after,
.row::after {
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

h2 {
  font-size: 26px;
}
</style>