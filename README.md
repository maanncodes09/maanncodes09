from PIL import Image, ImageDraw, ImageFont, ImageFilter
from pathlib import Path
import os, textwrap

src = Path("/mnt/data/animated_neon_cyberpunk_landscape.png")
out_dir = Path("/mnt/data/github_profile_redesign")
out_dir.mkdir(exist_ok=True)

im = Image.open(src)
frames = []
for i in range(getattr(im, "n_frames", 1)):
    im.seek(i)
    frame = im.convert("RGB").resize((1280, 400), Image.Resampling.LANCZOS)

    # Add a restrained dark overlay so the typography remains readable.
    overlay = Image.new("RGBA", frame.size, (4, 6, 12, 88))
    frame = Image.alpha_composite(frame.convert("RGBA"), overlay)

    # Soft dark vignette behind the centered typography.
    vignette = Image.new("RGBA", frame.size, (0, 0, 0, 0))
    vd = ImageDraw.Draw(vignette)
    vd.rounded_rectangle((260, 70, 1020, 330), radius=22, fill=(3, 5, 10, 95))
    vignette = vignette.filter(ImageFilter.GaussianBlur(32))
    frame = Image.alpha_composite(frame, vignette)

    draw = ImageDraw.Draw(frame)
    font_path = "/usr/share/fonts/truetype/dejavu/DejaVuSans.ttf"
    bold_path = "/usr/share/fonts/truetype/dejavu/DejaVuSans-Bold.ttf"
    name_font = ImageFont.truetype(bold_path, 58)
    sub_font = ImageFont.truetype(font_path, 18)
    tag_font = ImageFont.truetype(bold_path, 15)

    # Centered title and restrained metadata.
    title = "MANSI AGARWAL"
    bbox = draw.textbbox((0, 0), title, font=name_font)
    tw = bbox[2] - bbox[0]
    draw.text(((1280 - tw) / 2, 132), title, font=name_font, fill=(247, 248, 252, 255),
              stroke_width=1, stroke_fill=(10, 12, 20, 180))

    subtitle = "BUILD  ·  SOLVE  ·  EVOLVE"
    bbox = draw.textbbox((0, 0), subtitle, font=sub_font)
    sw = bbox[2] - bbox[0]
    draw.text(((1280 - sw) / 2, 218), subtitle, font=sub_font, fill=(204, 194, 218, 255))

    meta = "SOFTWARE ENGINEERING  /  BACKEND SYSTEMS  /  FULL-STACK DEVELOPMENT"
    bbox = draw.textbbox((0, 0), meta, font=tag_font)
    mw = bbox[2] - bbox[0]
    draw.text(((1280 - mw) / 2, 270), meta, font=tag_font, fill=(157, 166, 188, 255))

    frames.append(frame.convert("P", palette=Image.Palette.ADAPTIVE, colors=128))

gif_path = out_dir / "mansi-animated-hero.gif"
frames[0].save(
    gif_path,
    save_all=True,
    append_images=frames[1:],
    duration=500,
    loop=0,
    optimize=True,
    disposal=2
)

readme = r'''<div align="center">

<img src="mansi-animated-hero.gif" width="100%" alt="Mansi Agarwal — Build, Solve, Evolve" />

<br/>

<sub>SOFTWARE ENGINEERING &nbsp; / &nbsp; BACKEND SYSTEMS &nbsp; / &nbsp; FULL-STACK DEVELOPMENT</sub>

</div>

<br/>

<table>
  <tr>
    <td width="56%" valign="top">

      <sub>01 / ABOUT</sub>
      <h2>Building things that matter.</h2>
      Computer Science undergraduate interested in backend engineering, full-stack development and solving practical problems through software.

      <br/><br/>
      <a href="https://github.com/maanncodes09?tab=repositories">EXPLORE MY WORK ↗</a>

    </td>
    <td width="44%" valign="top">

      <sub>02 / ALWAYS LEARNING</sub>
      <h3>Engineering, one layer deeper.</h3>
      Java & DSA<br/>
      Backend APIs<br/>
      Databases<br/>
      System design fundamentals

      <br/><br/>
      <sub>03 / OPEN TO</sub><br/>
      Collaboration · Internships · Interesting problems

    </td>
  </tr>
</table>

<br/>

## 04 / SELECTED WORK

<table>
  <tr>
    <td width="50%" valign="top">
      <sub>01 — FULL-STACK</sub>
      <h3>Personal Finance Dashboard</h3>
      A finance management application for tracking income, expenses, budgets and savings.
      <br/><br/>
      <code>React</code> <code>Node.js</code> <code>Express</code> <code>MongoDB</code>
      <br/><br/>
      <a href="https://github.com/maanncodes09?tab=repositories">FIND REPOSITORY ↗</a>
    </td>
    <td width="50%" valign="top">
      <sub>02 — FRONTEND ENGINEERING</sub>
      <h3>FlowBoard</h3>
      A Kanban-style task workspace with drag-and-drop, filters, labels and undo/redo.
      <br/><br/>
      <code>JavaScript</code> <code>HTML</code> <code>CSS</code>
      <br/><br/>
      <a href="https://github.com/maanncodes09/TaskFlow---Smart-Task-Management-System">VIEW REPOSITORY ↗</a>
    </td>
  </tr>
  <tr>
    <td width="50%" valign="top">
      <sub>03 — FULL-STACK</sub>
      <h3>Food²</h3>
      Restaurant application with menu browsing, reservations, authentication and API integration.
      <br/><br/>
      <code>MongoDB</code> <code>Express</code> <code>JavaScript</code>
      <br/><br/>
      <a href="https://github.com/maanncodes09?tab=repositories">FIND REPOSITORY ↗</a>
    </td>
    <td width="50%" valign="top">
      <sub>04 — IN DEVELOPMENT</sub>
      <h3>Enterprise Helpdesk</h3>
      Collaborative incident-management platform exploring ticket workflows, role-based access and backend architecture.
      <br/><br/>
      <code>MERN</code> <code>REST APIs</code> <code>Database Design</code>
      <br/><br/>
      <sub>IN DEVELOPMENT</sub>
    </td>
  </tr>
</table>

<br/>

## 05 / TOOLKIT

**LANGUAGES**<br/>
`Java` &nbsp; `JavaScript`

**FRONTEND**<br/>
`HTML` &nbsp; `CSS` &nbsp; `React` &nbsp; `Bootstrap`

**BACKEND**<br/>
`Node.js` &nbsp; `Express.js`

**DATABASES**<br/>
`MongoDB` &nbsp; `PostgreSQL`

**TOOLS**<br/>
`Git` &nbsp; `GitHub` &nbsp; `Postman`

<br/>

## 06 / ENGINEERING ACTIVITY

<div align="center">

<img src="https://github-readme-stats.vercel.app/api?username=maanncodes09&show_icons=true&hide_border=true&bg_color=0B0C12&title_color=C9B3D8&icon_color=8E78A8&text_color=D6D2DC" height="165" alt="GitHub statistics" />

<img src="https://github-readme-stats.vercel.app/api/top-langs/?username=maanncodes09&layout=compact&hide_border=true&bg_color=0B0C12&title_color=C9B3D8&text_color=D6D2DC" height="165" alt="Top languages" />

</div>

<br/>

## 07 / COMMUNITY & MILESTONES

- GirlScript Summer of Code 2026 — Contributor
- SheFi Season 16 — Scholarship recipient
- MongoDB Student Basic — Certification

<br/>

## 08 / FIND ME

<div align="center">

<a href="https://github.com/maanncodes09">GitHub</a>
&nbsp; · &nbsp;
<!-- Replace the placeholder below with your LinkedIn profile URL -->
<a href="https://www.linkedin.com/">LinkedIn</a>
&nbsp; · &nbsp;
<!-- Replace the placeholder below with your professional email address -->
<a href="mailto:YOUR_EMAIL@example.com">Email</a>

<br/><br/>

<sub>BUILD WITH INTENTION. KEEP GETTING BETTER.</sub>

</div>
'''

readme_path = out_dir / "README.md"
readme_path.write_text(readme, encoding="utf-8")

print(f"Created: {gif_path} ({gif_path.stat().st_size/1024:.1f} KB)")
print(f"Created: {readme_path} ({readme_path.stat().st_size/1024:.1f} KB)")
