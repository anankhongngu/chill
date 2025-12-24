<template>
    <div class="page-resume wrapper-full">
         <div class="info">
            <ul>
                <li>From <b>HCMC</b> - VietNam</li>
                <li>15/05/1997</li>
                <li>Tien Giang University</li>
            </ul>
        </div>
        <div class="name">
            <h1>LE TRUONG AN</h1>
            <div class="specialize">Frond-End Developer</div>
            <ul class="contact">
                <li>
                    <span>P</span> 0766662587
                </li>
                <li>
                    <span>E</span> truongan2700@gmail.com
                </li>
            </ul>
        </div>
        <div class="experience">
            <h2>PROJECTS</h2>
            <div class="item">
                <h4>Website shopping</h4>
                <div class="time">
                    <span>2024</span>
                </div>
                <div class="web">
                   <a class="text-link" href="https://doraemon-store.myharavan.com">Dora Baby</a>
                </div>
                <div class="des">
                    <p>
                        Business theme, design suitable for fields: clothing, shoes, accessories, fashion   
                    </p>
                    <p>
                        Theme has been optimized for SEO standards. Includes many features and is optimized for loading speed.
                    </p>
                </div>
            </div>
            <div class="item">
                <h4>Website shopping</h4>
                <div class="time">
                    <span>2025</span>
                </div>
                <div class="web">
                   <a class="text-link" href="https://doraemon-cosmetics.myharavan.com/">Dora Cosmetics</a>
                </div>
                <div class="des">
                    <p>
                        Business theme, design suitable for fields: cosmetics, perfumes. beauty, skin care. accessories, fashion.
                    </p>
                    <p>
                        Theme has been optimized for SEO standards. Includes many features and is optimized for loading speed.
                    </p>
                    <p>
                        Outstanding feature: Optimize Google Pagespeed
                    </p>
                </div>
            </div>
            <h2 class="skills">
                SKILLS
            </h2>
            <ul>
                <li>HTML</li>
                <li>CSS / SCSS</li>
                <li>Bootstrap</li>
                <li>Javascript / jQuery</li>
                <li>Vuejs</li>
                <li>Liquid</li>
                <li>Optimize Google Pagespeed</li>
                <li>Git</li>
            </ul>
        </div>
        <div class="project">
            <h2>EXPERIENCE</h2>
            <div class="item">
                <h4>Frond-end Developer</h4>
                <div class="time">
                    10/2019 - 2021
                </div>
                <div class="time">
                    Nhan Hoa Software Company
                </div>
                <div class="location">
                    Frond-end Developer
                </div>
                <div class="des">
                    <p>
                        Here, I take on the role of web design and editing.
                        Our team communicates with clients via Zalo, gathers their requirements, and I work on completing and refining the web design according to their needs.
                    </p>
                    <p>
                        I have participated in a Node.js and Vue.js programming course at CyberSoft Academy.
                    </p>
                </div>
            </div>

            <div class="item">
                <h4>Frond-end Developer</h4>
                <div class="time">
                    2021 - Currently
                </div>
                <div class="time">
                    Working at haravan technology joint stock company
                </div>
                <div class="location">
                    Frond-end Developer
                </div>
                <div class="des">
                    <p>Here, I handle front-end web edits through a ticketing system. The company uses the Liquid programming language, which is similar to Shopify. This has been a period where I continuously strive to learn and explore new knowledge to improve myself.</p>    
                </div>
            </div>
            
            <h2>MORE INFOMATION</h2>
            <div class="item">
                <h4>Self-study</h4>
                <div class="des">
                    <p>
                        I consider myself a quick learner and consistently complete tasks effectively. 
                        I am always eager to learn and improve, with a strong focus on building user-friendly, efficient products that meet the needs of users.
                    </p>
                    <p>Some projects using Vuejs: </p>
                    <ul class="style-ul text-left">  
                        <li><a href="https://github.com/anankhongngu/Todo-List">Todo List: https://github.com/anankhongngu/Todo-List</a></li>
                        <li><a href="https://github.com/anankhongngu/Todo-List">Youtube music + Manga GET API: https://github.com/anankhongngu/YOUTUBE-MANGA</a></li>
                    </ul>
                </div>
            </div>
            <div class="item">
                <h4>Interest</h4>
                <div class="des">
                    <p>Soccer, badminton, games</p>
                </div>
            </div>
        </div>
         <div class="avatar">
            <!-- <img src="https://i.pinimg.com/564x/eb/57/6f/eb576ff023487bcb1fa3ad61ee7b23ee.jpg" alt=""> -->
        </div>
        <div class="intro">
            <h2>INTRODUCE MYSELT</h2> 
            <p>I am a Front-end developer with over 4 years of experience in website interface development, performance optimization, and providing smooth user experiences. 
                Throughout my career, I have become proficient in technologies such as HTML, CSS, JavaScript, and popular libraries like Bootstrap and jQuery. 
                Additionally, I have explored and gained experience with Vue.js and am familiar with version control tools like Git.
            </p>
            
        </div>
    </div>
</template>
<script>
</script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100..900;1,100..900&display=swap');
.page-resume {
    font-family: "Montserrat", sans-serif;
} 
.wrapper-full {
    max-width: 100%;
    padding: 50px;
    box-shadow: 0 10px 50px #858585;
    display: grid;
    grid-template-columns: 35% 60%;
    column-gap: 5%;
    row-gap: 30px;
    margin: 15px;
    height: calc(100% - 15px);
    color: var(--text-black);
    background-color: var(--white);
}
.avatar img{
    width:200px;
    height:200px;
    object-fit: cover;
    border-radius: 50%;
}
.avatar{
    display: flex;
    justify-content: end;
    align-items: center;
}
.name{
    padding-left: 30px;
    border-left:1px solid var(--black);
}
.name h1{
    font-weight: 300;
    font-size: xxx-large;
    margin:0px 0 5px ;
}
.name .specialize{
    margin:0;
    font-weight: bold;
    letter-spacing: 3px;
}
.name .contact{
    padding:0;
    list-style: none;
}
.name .contact span{
    display: inline-block;
    background-color: var(--black);
    width:20px;
    height:20px;
    color:var(--text-white);
    text-align: center;
    border-radius: 50%;
    font-size: small;
    margin-right: 20px;
}
.name .contact li{
    padding:5px 0;
    transform: translate(-40px);
}
.info ul{
    padding: 0;
    margin: 0;
    list-style: none;
    text-align: right;
    line-height: 30px;
} 

h2{
    padding:0;
    margin: 0 0 20px 0;
}
.intro{
    padding-left: 30px;
    border-left: 1px solid var(--black);
    text-align: left;
}
.experience{
    text-align: right;
}
.experience h4{
    margin:20px 0 10px;
}
.skills{
    margin-top:  50px;
}
.experience ul{
    padding:0;
    margin: 0;
list-style: none;
}
.experience li{
    padding:10px 0;
}
.project{
    padding-left: 30px;
    text-align: left;
    border-left: 1px solid var(--black);
}
.project h4{
    margin:20px 0 10px;
}
.text-link{
    color: var(--icon-green);
    font-weight: bold;display: block;
}
.project .location{
    font-weight: bold;
    padding: 10px 0;
}
.project .item{
    position: relative;
}
.project .item::before{
    position: absolute;
    width:13px;
    height: 13px;
    background-color: var(--black);
    content: '';
    border-radius: 50%;
    left: -37px;
}
.style-ul li {
    list-style: inside;
    padding-left: 25px;
    margin-top: 12px;
}
.style-ul li a {
    color: var(--icon-green);
}
</style>