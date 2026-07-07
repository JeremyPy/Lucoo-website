---
title: "Lucoo"
description: "数据驱动的 VPS 与主机测评、对比与推荐"
---

<div style="margin-top: 1rem; text-align: center;">
  <div class="home-contact-links">
    <span>QQ群：1095751478</span>
    <button type="button" class="home-qq-trigger" data-lucoo-qq-open>
      加入QQ交流群：1081378039
    </button>
    <a href="https://t.me/+QpGWaohfpUw3ZDY1" target="_blank" rel="noopener noreferrer">
      TG群：https://t.me/+QpGWaohfpUw3ZDY1
    </a>
  </div>
  <div class="home-action-links">
    <a href="https://api.lucoo.net" target="_blank" rel="noopener noreferrer" class="home-action-button home-action-relay">
      中转站地址：https://api.lucoo.net
    </a>
    <a href="https://pay.ldxp.cn/shop/Lucoo" target="_blank" rel="noopener noreferrer" class="home-action-button home-action-shop">
      店铺地址：https://pay.ldxp.cn/shop/Lucoo
    </a>
    <button type="button" class="home-action-button home-action-qq" data-lucoo-qq-open>
      加入QQ交流群：1081378039
    </button>
  </div>
</div>

<div id="lucoo-qq-modal" class="lucoo-qq-modal" hidden aria-hidden="true" role="dialog" aria-modal="true" aria-labelledby="lucoo-qq-modal-title">
  <div class="lucoo-qq-modal__backdrop" data-lucoo-qq-close></div>
  <div class="lucoo-qq-modal__dialog" role="document">
    <button type="button" class="lucoo-qq-modal__close" data-lucoo-qq-close aria-label="关闭 QQ 群二维码弹窗">×</button>
    <div class="lucoo-qq-modal__header">
      <div class="lucoo-qq-modal__eyebrow">Lucoo 社群</div>
      <h2 id="lucoo-qq-modal-title">加入 QQ 交流群：1081378039</h2>
      <p>扫码加入群聊，或点击下方链接直接跳转加入。</p>
    </div>
    <img class="lucoo-qq-modal__qrcode" src="/images/qq-lucoo-ai-group-qrcode.png" alt="Lucoo人工智能交流群 QQ 群二维码，群号 1081378039" decoding="async" fetchpriority="low">
    <a class="lucoo-qq-modal__join" href="https://qm.qq.com/q/WXscESbS0w" target="_blank" rel="noopener noreferrer">
      点击链接加入群聊【Lucoo人工智能交流群1️⃣】
    </a>
    <p class="lucoo-qq-modal__hint">如果链接打不开，可以打开 QQ 扫描上方二维码。</p>
  </div>
</div>

<script>
  (function () {
    var modal = document.getElementById('lucoo-qq-modal');
    if (!modal) return;

    var openButtons = document.querySelectorAll('[data-lucoo-qq-open]');
    var closeButtons = modal.querySelectorAll('[data-lucoo-qq-close]');
    var lastActive = null;

    function openModal() {
      lastActive = document.activeElement;
      modal.hidden = false;
      modal.setAttribute('aria-hidden', 'false');
      document.documentElement.classList.add('lucoo-modal-open');
      var closeButton = modal.querySelector('.lucoo-qq-modal__close');
      if (closeButton) closeButton.focus();
    }

    function closeModal() {
      modal.hidden = true;
      modal.setAttribute('aria-hidden', 'true');
      document.documentElement.classList.remove('lucoo-modal-open');
      if (lastActive && typeof lastActive.focus === 'function') lastActive.focus();
    }

    openButtons.forEach(function (button) {
      button.addEventListener('click', openModal);
    });

    closeButtons.forEach(function (button) {
      button.addEventListener('click', closeModal);
    });

    document.addEventListener('keydown', function (event) {
      if (event.key === 'Escape' && !modal.hidden) closeModal();
    });
  })();
</script>
