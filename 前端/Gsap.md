[GSAP 入门 - 学习中心 - GreenSock](https://greensock.com/get-started#transformShorthand)

https://gsap.framer.wiki/ 中文文档



# 触发函数

~~~js
function myFunction() {
  console.log("The animation has started!");
}

var myTween = TweenMax.to(".my-element", 1, {
  opacity: 0,
  onStart: myFunction
})
~~~







# ScrollTrigger

~~~js
gsap.to(".box", {
  scrollTrigger: ".box", // 当 ".box" 进入窗口时开始动画
  x: 500
});
~~~



~~~js
let tl = gsap.timeline({
    // 可以加入到时间条
    scrollTrigger: {
      trigger: ".container",
      pin: true,   // 激活时固定触发元件
      start: "top top", // 当触发器的顶部碰到视口的顶部时
      end: "+=500", // e滚动500px后结束
      scrub: 1, // 平滑滚动，需要 1 秒赶上滚动条
      snap: {
        snapTo: "labels", // 捕捉到时间轴中最接近的标签
        duration: {min: 0.2, max: 3}, // 快照动画应该至少0.2秒，但不超过3秒(由速度决定)
        delay: 0.2, // 从最后一个滚动事件开始等待0.2秒，然后再进行抓拍
        ease: "power1.inOut"
      }
    }
  });

// 向时间轴添加动画和标签
tl.addLabel("start")
  .from(".box p", {scale: 0.3, rotation:45, autoAlpha: 0})
  .addLabel("color")
  .from(".box", {backgroundColor: "#28a92b"})
  .addLabel("spin")
  .to(".box", {rotation: 360})
  .addLabel("end");
~~~

