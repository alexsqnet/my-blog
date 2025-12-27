---
title: My Projects
date: 2025-12-27 00:27:58
layout: page
---

## My Projects

<hr>

### 1. Muzza (Desktop App)

<div style="display: flex; gap: 16px; align-items: flex-start; margin: 16px 0;">
  <img src="/uploads/projects/muzza/images/Muzza.png" alt="Muzza screenshot" data-zoom="none" style="width: 200px; border-radius: 4px; box-shadow: 0 2px 8px rgba(0,0,0,0.15); cursor: default; margin: 0;">
  <div>
    <strong>Summary:</strong> Text editor to compose lyrics, texts or notes. Load the song, relax and write!
    <br>
    <strong>Platform:</strong> Windows / Linux (soon)
    <br>
    <strong>License:</strong> Free (no hidden costs)
    <br>
    <strong>Current version:</strong> 1.0.0
    <br>
    <strong>SHA256:</strong> C8C2F16A409A7A1F828A1F996DE72385C86CA4B4748DEBCBC3CD4DF56A03CBB5
    <br>
    <a href="https://drive.google.com/file/d/1RmJO_MlQe598EhNJQfnMCQ6Fjcat_161/view?usp=drive_link"><strong>Download ZIP</strong></a>
  </div>
</div>

#### Screenshots
<div style="display: flex; gap: 12px; flex-wrap: wrap; margin: 12px 0;">
  <img class="project-shot" src="/uploads/projects/muzza/images/screenshot1.png" data-full="/uploads/projects/muzza/images/screenshot1.png" alt="Muzza screenshot 1" style="width: 260px; border-radius: 4px; box-shadow: 0 2px 8px rgba(0,0,0,0.12); cursor: pointer;" data-zoom="none">
  <img class="project-shot" src="/uploads/projects/muzza/images/screenshot2.png" data-full="/uploads/projects/muzza/images/screenshot2.png" alt="Muzza screenshot 2" style="width: 260px; border-radius: 4px; box-shadow: 0 2px 8px rgba(0,0,0,0.12); cursor: pointer;" data-zoom="none">
  <img class="project-shot" src="/uploads/projects/muzza/images/screenshot3.png" data-full="/uploads/projects/muzza/images/screenshot3.png" alt="Muzza screenshot 3" style="width: 260px; border-radius: 4px; box-shadow: 0 2px 8px rgba(0,0,0,0.12); cursor: pointer;" data-zoom="none">
</div>

<div id="shot-modal" style="display:none; position:fixed; inset:0; background:rgba(0,0,0,0.75); align-items:center; justify-content:center; padding:24px; z-index:9999;">
  <div style="position:relative; max-width:90vw; max-height:90vh;">
    <button id="shot-modal-close" aria-label="Close" style="position:absolute; top:-10px; right:-10px; background:#fff; border:none; border-radius:50%; width:32px; height:32px; cursor:pointer; box-shadow:0 2px 8px rgba(0,0,0,0.25);">×</button>
    <img id="shot-modal-img" src="" alt="Screenshot preview" style="max-width:90vw; max-height:90vh; border-radius:6px; box-shadow:0 6px 24px rgba(0,0,0,0.35);">
  </div>
</div>

<script>
  // Simple modal preview for project screenshots.
  (function() {
    var modal = document.getElementById('shot-modal');
    var modalImg = document.getElementById('shot-modal-img');
    var closeBtn = document.getElementById('shot-modal-close');
    if (!modal || !modalImg) return;

    function openModal(src, alt) {
      modalImg.src = src;
      modalImg.alt = alt || 'Screenshot preview';
      modal.style.display = 'flex';
    }
    function closeModal() {
      modal.style.display = 'none';
      modalImg.src = '';
    }
    Array.prototype.forEach.call(document.querySelectorAll('.project-shot'), function(img) {
      img.addEventListener('click', function() {
        openModal(img.getAttribute('data-full') || img.src, img.alt);
      });
    });
    modal.addEventListener('click', function(e) {
      if (e.target === modal) closeModal();
    });
    if (closeBtn) closeBtn.addEventListener('click', closeModal);
  })();
</script>

<hr>


## Want something specific?
If you need a custom build or run into issues, email me at [alexandr.sco@gmail.com](mailto:alexandr.sco@gmail.com).
