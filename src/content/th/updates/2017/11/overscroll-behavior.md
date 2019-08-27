project_path: "/web/_project.yaml"
book_path: "/web/updates/_book.yaml"
description: ข้อมูลเบื้องต้นเกี่ยวกับคุณสมบัติ CSS การทำงานเกินขนาด

{# wf_updated_on: 2019-08-26 #} {# wf_published_on: 2017-11-14 #}

{# wf_tags: chrome63,css,overscroll,scroll #} {# wf_blink_components: Blink>CSS
#} {# wf_featured_image:/web/updates/images/2017/11/overscroll-behavior/card.png
#} {# wf_featured_snippet: The CSS overscroll-behavior property allows
developers to override the browser's overflow scroll effects when reaching the
top/bottom of content. It can be used to customize or prevent the mobile
pull-to-refresh action. #}

# ควบคุมการเลื่อนของคุณ: การกำหนดเอฟเฟกต์แบบดึงเพื่อรีเฟรชและโอเวอร์โฟลว์ {: .page-title }

{% include "web/_shared/contributors/ericbidelman.html" %} {% include
"web/_shared/contributors/majidvp.html" %} {% include
"web/_shared/contributors/sunyunjia.html" %}

<style>
figure {
  text-align: center;
}
figcaption {
  font-size: 14px;
  font-style: italic;
}
.border {
  border: 1px solid #ccc;
}
.centered {
  display: flex;
  justify-content: center;
}
</style>

### TL; DR {: #tldr .hide-from-toc}

คุณสมบัติ [CSS
`overscroll-behavior`](https://wicg.github.io/overscroll-behavior/)
ช่วยให้นักพัฒนาสามารถแทนที่พฤติกรรมการเลื่อนล้นเริ่มต้นของเบราว์เซอร์เมื่อไปถึงด้านบน
/ ล่างของเนื้อหา
กรณีการใช้งานรวมถึงการปิดใช้งานคุณสมบัติดึงเพื่อรีเฟรชบนมือถือการลบการเรืองแสงที่มากเกินไปและเอฟเฟกต์แถบยางและการป้องกันไม่ให้เนื้อหาของหน้าเลื่อนเมื่ออยู่ภายใต้คำกริยา
/ ภาพซ้อนทับ

`overscroll-behavior` ต้องการ Chrome 63+
มันอยู่ในการพัฒนาหรือการพิจารณาโดยเบราว์เซอร์อื่น ๆ ดู
[chromestatus.com](https://www.chromestatus.com/feature/5734614437986304)
สำหรับข้อมูลเพิ่มเติม {: .caution }

## พื้นหลัง

### เลื่อนขอบเขตและเลื่อนการผูกมัด {: #scrollchaining }

<figure class="attempt-right">
<a href="/web/updates/images/2017/11/overscroll-behavior/drawer-scroll.mp4"
target="_blank">
<video
src="/web/updates/images/2017/11/overscroll-behavior/drawer-scroll.mp4" autoplay
loop muted alt="Drawer demo" height="300"></video>
  </a>
  <figcaption>เลื่อนการผูกมัดบน Chrome Android</figcaption>
</figure>

การเลื่อนเป็นหนึ่งในวิธีพื้นฐานที่สุดในการโต้ตอบกับหน้า แต่รูปแบบ UX
บางอย่างอาจยุ่งยากในการจัดการเนื่องจากพฤติกรรมเริ่มต้นที่แปลกประหลาดของเบราว์เซอร์
ยกตัวอย่างเช่นใช้แอป drawer ที่มีไอเท็มจำนวนมากที่ผู้ใช้อาจต้องเลื่อนดู
เมื่อถึงด้านล่างคอนเทนเนอร์โอเวอร์โฟลว์หยุดเลื่อนเนื่องจากไม่มีเนื้อหาที่ต้องใช้อีก
กล่าวอีกนัยหนึ่งผู้ใช้มาถึง "ขอบเขตการเลื่อน"
แต่ให้สังเกตว่าจะเกิดอะไรขึ้นหากผู้ใช้ยังคงเลื่อน **เนื้อหา *ด้านหลัง*
ลิ้นชักเริ่มเลื่อน** ! การเลื่อนถูกครอบครองโดยคอนเทนเนอร์พาเรนต์
หน้าหลักของตัวเองในตัวอย่าง

ปรากฎว่าพฤติกรรมนี้เรียกว่าการ **ผูกมัดแบบเลื่อน**
พฤติกรรมเริ่มต้นของเบราว์เซอร์เมื่อเลื่อนเนื้อหา
บ่อยครั้งที่ค่าเริ่มต้นค่อนข้างดี แต่บางครั้งก็ไม่เป็นที่ต้องการหรือไม่คาดคิด
แอพบางตัวอาจต้องการมอบประสบการณ์การใช้งานที่แตกต่างเมื่อผู้ใช้พบกับขอบเขตการเลื่อน

### เอฟเฟกต์แบบดึงเพื่อรีเฟรช {: #p2r }

Pull-to-refresh เป็นท่าทางที่ใช้งานง่ายซึ่งเป็นที่นิยมโดยแอพมือถือเช่น Facebook
และ Twitter ดึงลงในฟีดทางสังคมและปล่อยสร้างพื้นที่ใหม่สำหรับการโหลดโพสต์ล่าสุด
ในความเป็นจริง UX นี้ได้กลายเป็นที่ *นิยมอย่างมาก* จนเบราว์เซอร์มือถือเช่น
Chrome บน Android ได้ใช้เอฟเฟกต์เดียวกัน ปัดลงที่ด้านบนของหน้ารีเฟรชทั้งหน้า:

<div class="clearfix centered">
  <figure class="attempt-left">
<a href="/web/updates/images/2017/11/overscroll-behavior/twitter.mp4"
target="_blank">
<video src="/web/updates/images/2017/11/overscroll-behavior/twitter.mp4"
autoplay muted loop height="350" class="border"></video>
    </a>
<figcaption>การดึงเพื่อรีเฟรชของ Twitter เอง <br> เมื่อรีเฟรชฟีดใน
PWA</figcaption>
  </figure>
  <figure class="attempt-right">
<a href="/web/updates/images/2017/11/overscroll-behavior/mobilep2r.mp4"
target="_blank">
<video
src="/web/updates/images/2017/11/overscroll-behavior/mobilep2r.mp4" autoplay
muted loop height="350" class="border"></video>
    </a>
<figcaption>การทำงานแบบลากเพื่อรีเฟรชของ Chrome Android <br>
รีเฟรชทั้งหน้า</figcaption>
  </figure>
</div>

สำหรับสถานการณ์เช่น Twitter [PWA](/web/progressive-web-apps/)
อาจทำให้การปิดใช้งานการกระทำแบบดึงเพื่อรีเฟรช ทำไม?
ในแอพนี้คุณอาจไม่ต้องการให้ผู้ใช้รีเฟรชหน้าโดยไม่ตั้งใจ
นอกจากนี้ยังมีศักยภาพที่จะเห็นภาพเคลื่อนไหวการรีเฟรชสองครั้ง!
หรืออาจจะดีกว่าในการปรับแต่งการทำงานของเบราว์เซอร์โดยปรับให้สอดคล้องกับการสร้างแบรนด์ของไซต์
ส่วนที่โชคร้ายคือการปรับแต่งประเภทนี้ยากที่จะดึงออกมา
นักพัฒนาจะจบลงด้วยการเขียนที่ไม่จำเป็น JavaScript เพิ่ม [ไม่ใช่เรื่อย
ๆ](/web/tools/lighthouse/audits/passive-event-listeners) ผู้ฟังสัมผัส
(ซึ่งกันโดยเลื่อน) หรือติดหน้าทั้งหมดใน 100vw / VH `<div>`
(เพื่อป้องกันหน้าจากล้น) วิธีแก้ปัญหาเหล่านี้มีผลกระทบเชิงลบที่มีการ
[บันทึกไว้อย่างดี](https://wicg.github.io/overscroll-behavior/#intro)
เกี่ยวกับประสิทธิภาพการเลื่อน

เราทำได้ดีกว่า!

## การแนะนำ `overscroll-behavior` {: #intro }

คุณสมบัติ `overscroll-behavior` เป็น
[คุณสมบัติ](https://wicg.github.io/overscroll-behavior/) CSS
ใหม่ที่ควบคุมพฤติกรรมของสิ่งที่เกิดขึ้นเมื่อคุณเลื่อนคอนเทนเนอร์
(รวมถึงหน้าตัวเอง) คุณสามารถใช้มันเพื่อยกเลิกการโยงเลื่อนปิดการใช้งาน /
ปรับแต่งการกระทำแบบดึงเพื่อรีเฟรชปิดการใช้งานเอฟเฟกต์แถบยางบน iOS (เมื่อ Safari
ใช้ `overscroll-behavior` เลื่อนเกิน) และอื่น ๆ ส่วนที่ดีที่สุดคือการ <strong
data-md-type="double_emphasis">ใช้การควบคุมการ `overscroll-behavior`
จะไม่ส่งผลเสียต่อประสิทธิภาพของหน้าเว็บ</strong> เช่นการแฮ็กที่กล่าวถึงในบทนำ!

คุณสมบัติใช้ค่าที่เป็นไปได้สามค่า:

1. **อัตโนมัติ** - เริ่มต้น
การเลื่อนที่เกิดขึ้นบนองค์ประกอบอาจแพร่กระจายไปยังองค์ประกอบที่บรรพบุรุษ

- **บรรจุ** - ป้องกันการโยงกระดาษม้วน การเลื่อนไม่แพร่กระจายไปยังบรรพบุรุษ
แต่จะแสดงเอฟเฟกต์ภายในโหนด ตัวอย่างเช่นเอฟเฟกต์การเรืองแสงแบบโอเวอร์ครอลใน
Android หรือเอฟเฟกต์แถบยางบน iOS
ซึ่งจะแจ้งเตือนผู้ใช้เมื่อพวกเขาถึงขอบเขตการเลื่อน **หมายเหตุ** : การใช้
`overscroll-behavior: contain` ในองค์ประกอบ `html` ป้องกันการกระทำการนำทาง
overscroll
- **ไม่มี** - เหมือนกับที่ `contain`
แต่มันยังป้องกันเอฟเฟกต์การเลื่อนเกินในโหนดเอง (เช่นการโอเวอร์คล็อก Android
หรือแถบยางของ iOS)

หมายเหตุ: `overscroll-behavior` ยังสนับสนุนการจดชวเลขสำหรับ
`overscroll-behavior-x` และ `overscroll-behavior-y`
หากคุณต้องการกำหนดพฤติกรรมเฉพาะสำหรับแกนที่แน่นอน

ลองดำดิ่งลงไปในตัวอย่างเพื่อดูวิธีการใช้งานเกิน `overscroll-behavior`

## ป้องกันไม่ให้เลื่อนจากองค์ประกอบตำแหน่งคงที่ {: #fixedpos }

### สถานการณ์แชทบ็อกซ์ {: #chat }

<figure class="attempt-right">
<a href="/web/updates/images/2017/11/overscroll-behavior/chatbox-chaining.mp4"
target="_blank">
<video
src="/web/updates/images/2017/11/overscroll-behavior/chatbox-chaining.mp4"
autoplay muted loop alt="Chatbox demo" height="350" class="border"></video>
  </a>
  <figcaption>เนื้อหาที่อยู่ใต้หน้าต่างแชทจะเลื่อนด้วย :(</figcaption>
</figure>

พิจารณาแชทที่อยู่ในตำแหน่งคงที่ซึ่งอยู่ที่ด้านล่างของหน้า
จุดประสงค์คือกล่องแชทเป็นองค์ประกอบในตัวและเลื่อนแยกออกจากเนื้อหาที่อยู่ด้านหลัง
อย่างไรก็ตามเนื่องจากการผูกมัดการเลื่อนเอกสารเริ่มการเลื่อนทันทีที่ผู้ใช้พบข้อความล่าสุดในประวัติการแชท

สำหรับแอปนี้การเลื่อนที่มาจากกล่องแชทจะเหมาะสมกว่าในการแชท
เราสามารถทำให้เกิดขึ้นได้โดยการเพิ่ม `overscroll-behavior: contain`
ในองค์ประกอบที่เก็บข้อความแชท:

```css
#chat .msgs {
  overflow: auto;
  overscroll-behavior: contain;
  height: 300px;
}
```

โดยพื้นฐานแล้วเรากำลังสร้างการแยกเชิงตรรกะระหว่างบริบทการเลื่อนของกล่องแชทกับหน้าหลัก
ผลลัพธ์ที่ได้คือหน้าหลักจะยังคงอยู่เมื่อผู้ใช้ไปถึงด้านบน / ล่างของประวัติการแชท
การเลื่อนที่เริ่มต้นในกล่องแชทจะไม่แพร่กระจายออกไป

### สถานการณ์การซ้อนทับหน้า {: #overlay }

การเปลี่ยนแปลงของสถานการณ์ "underscroll"
**ก็คือเมื่อคุณเห็นการเลื่อนเนื้อหาที่อยู่เบื้องหลังการซ้อนทับตำแหน่งคงที่** แจก
`overscroll-behavior` ตาย `overscroll-behavior` ในการสั่งซื้อ!
เบราว์เซอร์พยายามเป็นประโยชน์ แต่ท้ายที่สุดแล้วการทำให้เว็บไซต์ดูเป็นรถ

**ตัวอย่าง** - เป็นกิริยาช่วยที่มีและไม่มี `overscroll-behavior: contain` :

<figure class="clearfix centered">
  <div class="attempt-left">
<a href="/web/updates/images/2017/11/overscroll-behavior/modal-off.mp4"
target="_blank">
<video src="/web/updates/images/2017/11/overscroll-behavior/modal-off.mp4"
autoplay muted loop height="290"></video>
    </a>
<figcaption><b>ก่อน</b> หน้า:
เนื้อหาของหน้าเลื่อนอยู่ใต้ภาพซ้อนทับ</figcaption>
  </div>
  <div class="attempt-right">
<a href="/web/updates/images/2017/11/overscroll-behavior/modal-on.mp4"
target="_blank">
<video src="/web/updates/images/2017/11/overscroll-behavior/modal-on.mp4"
autoplay muted loop height="290"></video>
    </a>
<figcaption><b>หลัง</b> :
เนื้อหาของหน้าไม่เลื่อนลงใต้ภาพซ้อนทับ</figcaption>
  </div>
</figure>

## ปิดการใช้งาน pull-to-refresh {: #disablp2r }

**การปิดการทำงานแบบดึงเพื่อรีเฟรชเป็น CSS บรรทัดเดียว**
เพียงแค่ป้องกันการผูกมัดการเลื่อนบนองค์ประกอบที่กำหนดวิวพอร์ตทั้งหมด
ในกรณีส่วนใหญ่นั่นคือ `<html>` หรือ `<body>` :

```css
body {
  /* Disables pull-to-refresh but allows overscroll glow effects. */
  overscroll-behavior-y: contain;
}
```

ด้วยการเพิ่มที่เรียบง่ายนี้เราแก้ไขภาพเคลื่อนไหวแบบ pull-to-refresh
สองครั้งในการ [สาธิตแชทบ็อกซ์](https://ebidel.github.io/demos/chatbox.html)
และสามารถใช้เอฟเฟกต์แบบกำหนดเองซึ่งใช้ภาพเคลื่อนไหวการโหลดแบบ neater แทน
กล่องจดหมายทั้งหมดยังเบลอเมื่อกล่องจดหมายรีเฟรช:

<figure class="clearfix centered">
  <div class="attempt-left">
<video
src="/web/updates/images/2017/11/overscroll-behavior/chatbox-double-refresh.mp4"
autoplay muted loop height="225"></video>
    <figcaption>ก่อน</figcaption>
  </div>
  <div class="attempt-right">
<video
src="/web/updates/images/2017/11/overscroll-behavior/chatbox-double-refresh-fix.mp4"
autoplay muted loop height="225"></video>
    <figcaption>หลังจาก</figcaption>
  </div>
</figure>

นี่คือตัวอย่างของ
[รหัสเต็ม](https://github.com/ebidel/demos/blob/master/chatbox.html) :

```html
<style>
  body.refreshing #inbox {
    filter: blur(1px);
    touch-action: none; /* prevent scrolling */
  }
  body.refreshing .refresher {
    transform: translate3d(0,150%,0) scale(1);
    z-index: 1;
  }
  .refresher {
    --refresh-width: 55px;
    pointer-events: none;
    width: var(--refresh-width);
    height: var(--refresh-width);
    border-radius: 50%; 
    position: absolute;
    transition: all 300ms cubic-bezier(0,0,0.2,1);
    will-change: transform, opacity;
    ...
  }
</style>

<div class="refresher">
  <div class="loading-bar"></div>
  <div class="loading-bar"></div>
  <div class="loading-bar"></div>
  <div class="loading-bar"></div>
</div>

<section id="inbox"><!-- msgs --></section>

<script>
  let _startY;
  const inbox = document.querySelector('#inbox');

  inbox.addEventListener('touchstart', e => {
    _startY = e.touches[0].pageY;
  }, {passive: true});

  inbox.addEventListener('touchmove', e => {
    const y = e.touches[0].pageY;
    // Activate custom pull-to-refresh effects when at the top of the container
    // and user is scrolling up.
    if (document.scrollingElement.scrollTop === 0 && y > _startY &&
        !document.body.classList.contains('refreshing')) {
      // refresh inbox.
    }
  }, {passive: true});
</script>
```

## การปิดใช้งานการแสดงผลโอเวอร์ครอลและเอฟเฟกต์แถบยาง {: #disableglow }

หากต้องการปิดใช้งานเอฟเฟกต์ตีกลับเมื่อกดปุ่มขอบเขตการเลื่อนให้ใช้
`overscroll-behavior-y: none` :

```css
body {
  /* Disables pull-to-refresh and overscroll glow effect.
     Still keeps swipe navigations. */
  overscroll-behavior-y: none;
}
```

<figure class="clearfix centered">
  <div class="attempt-left">
<video src="/web/updates/images/2017/11/overscroll-behavior/drawer-glow.mp4"
autoplay muted loop height="300" class="border"></video>
<figcaption><b>ก่อนหน้า</b> :
การกดปุ่มขอบเขตการเลื่อนจะเป็นการเรืองแสง</figcaption>
  </div>
  <div class="attempt-right">
<video
src="/web/updates/images/2017/11/overscroll-behavior/drawer-noglow.mp4" autoplay
muted loop height="300" class="border"></video>
    <figcaption><b>หลัง</b> : เรืองแสงถูกปิดใช้งาน</figcaption>
  </div>
</figure>

หมายเหตุ: วิธีนี้จะยังคงรักษาการนำทางปัดไปทางซ้าย / ขวา
เพื่อป้องกันการนำทางคุณสามารถใช้ `overscroll-behavior-x: none` อย่างไรก็ตาม
[ยังคงมีการใช้งาน](https://crbug.com/762023) ใน Chrome

## การสาธิตแบบเต็ม {: #demo }

การรวมทั้งหมดเข้าด้วยกันการ [สาธิต](https://ebidel.github.io/demos/chatbox.html)
แบบเต็ม `overscroll-behavior` จะใช้ `overscroll-behavior`
เพื่อสร้างภาพเคลื่อนไหวแบบดึงเพื่อรีเฟรชที่กำหนดเองและปิดใช้งานการเลื่อนจากการหลบหนีวิดเจ็ตแชท
สิ่งนี้มอบประสบการณ์การใช้งานที่ดีที่สุดซึ่งจะยุ่งยากหากไม่ใช้ CSS เกิน
`overscroll-behavior`

<figure>
  <a href="https://ebidel.github.io/demos/chatbox.html" target="_blank">
<video
src="/web/updates/images/2017/11/overscroll-behavior/chatbox-fixed.mp4" autoplay
muted loop alt="Chatbox demo" height="600"></video>
  </a>
<figcaption><a href="https://ebidel.github.io/demos/chatbox.html"
target="_blank">ดูการสาธิต</a> <a
href="https://github.com/ebidel/demos/blob/master/chatbox.html"
target="_blank">แหล่ง</a></figcaption>
</figure>

<br>
