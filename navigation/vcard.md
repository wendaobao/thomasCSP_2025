---
layout: tailwind
title: V-Card
description: Digital business card with contact information
permalink: /vcard
---

<!-- Tailwind CDN inject -->
<script src="https://cdn.tailwindcss.com"></script>

<div class="min-h-screen bg-gradient-to-br from-blue-50 to-white flex flex-col items-center justify-center px-6 py-12 space-y-10">

  <!-- Profile Section -->
  <div class="flex flex-col items-center space-y-4">
    <img src="https://i.postimg.cc/4yDYy3Y9/IMG-5001-2.jpg" alt="Wendao Bao" class="w-32 h-32 rounded-xl shadow-lg object-cover" />
    <h1 class="text-2xl font-bold text-gray-800">Wendao Bao</h1>
    <p class="text-sm text-gray-600">Developer • Student • Creator</p>
  </div>

  <!-- Cards Grid -->
  <div class="grid grid-cols-1 md:grid-cols-2 gap-8 w-full max-w-4xl">

    <!-- Contact Info Card -->
    <div class="bg-white rounded-2xl border border-gray-200 shadow-lg p-6 flex flex-col justify-between">
      <h2 class="text-xl font-semibold text-gray-800 mb-4">Contact Information</h2>
      <ul class="space-y-3 text-gray-700 text-sm">
        <li class="flex items-center gap-2">
          <img src="https://img.icons8.com/color/24/gmail.png" />
          <a href="mailto:wendaobao@gmail.com" class="hover:underline">wendaobao@gmail.com</a>
        </li>
        <li class="flex items-center gap-2">
          <img src="https://img.icons8.com/material-outlined/24/github.png" />
          <a href="https://github.com/wendaobao" target="_blank" class="hover:underline">GitHub</a>
        </li>
        <li class="flex items-center gap-2">
          <img src="https://img.icons8.com/color/24/linkedin.png" />
          <a href="https://www.linkedin.com/in/wendao-bao-225180368/" target="_blank" class="hover:underline">LinkedIn</a>
        </li>
        <li class="flex items-center gap-2">
          <img src="https://img.icons8.com/color/24/instagram-new.png" />
          <a href="https://www.instagram.com/therealthomasbao" target="_blank" class="hover:underline">Instagram</a>
        </li>
      </ul>
      <div class="pt-6 text-center">
        <button onclick="downloadVCard()" class="bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-md shadow-md hover:shadow-lg transition-transform transform hover:scale-105">
          📄 Download vCard
        </button>
      </div>
    </div>

    <!-- QR Code Card -->
    <div class="bg-white rounded-2xl border border-gray-200 shadow-lg p-6 flex flex-col items-center justify-center space-y-6">
      <h2 class="text-xl font-semibold text-gray-800 text-center">Quick Connect</h2>
      <div class="text-center">
        <img src="https://api.qrserver.com/v1/create-qr-code/?size=180x180&data=https://wendaobao.github.io/vcard" alt="QR for this page" class="rounded-lg shadow border border-gray-200" />
        <p class="text-sm text-gray-500 mt-2">This Page</p>
      </div>
      <div class="text-center">
        <img src="https://api.qrserver.com/v1/create-qr-code/?size=180x180&data=https://wendaobao.github.io" alt="QR for website" class="rounded-lg shadow border border-gray-200" />
        <p class="text-sm text-gray-500 mt-2">Portfolio Website</p>
      </div>
    </div>

  </div>
</div>

<script>
function downloadVCard() {
  const vCardData = `BEGIN:VCARD
VERSION:3.0
FN:Wendao Bao
EMAIL:wendaobao@gmail.com
URL:https://github.com/wendaobao
NOTE:Connect with me via LinkedIn, GitHub, or check out my portfolio!
END:VCARD`;

  const blob = new Blob([vCardData], { type: 'text/vcard' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = 'wendao-bao.vcf';
  document.body.appendChild(a);
  a.click();
  document.body.removeChild(a);
  URL.revokeObjectURL(url);
}
</script>
