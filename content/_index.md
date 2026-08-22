---
title: "The Compute Lab"
layout: hextra-home
---

<div style="
  display: flex;
  justify-content: flex-start;
  margin: 0.5rem 0 1.5rem 0;
  padding-left: 0.25rem;
">

  <a href="/docs/" class="quantum-docs-button">
    <span class="quantum-button-dot"></span>
    <span>Explore the Docs</span>
    <span class="quantum-arrow">→</span>
  </a>

</div>

{{< quantum-animation >}}

<style>
.quantum-docs-button {
  display: inline-flex;
  align-items: center;
  gap: 0.55rem;

  padding: 0.48rem 0.9rem;

  border: 1px solid rgba(245, 158, 11, 0.65);
  border-radius: 999px;

  background: rgba(20, 18, 15, 0.75);

  color: #fcd34d;

  font-size: 0.78rem;
  font-weight: 500;
  letter-spacing: 0.04em;

  text-decoration: none;

  box-shadow:
    0 0 8px rgba(245, 158, 11, 0.12),
    inset 0 0 10px rgba(245, 158, 11, 0.04);

  transition:
    transform 0.25s ease,
    box-shadow 0.25s ease,
    border-color 0.25s ease,
    color 0.25s ease;
}

.quantum-docs-button:hover {
  color: #fde68a;
  border-color: #fbbf24;

  transform: translateY(-1px);

  box-shadow:
    0 0 14px rgba(245, 158, 11, 0.28),
    0 0 30px rgba(245, 158, 11, 0.08),
    inset 0 0 12px rgba(245, 158, 11, 0.06);
}

.quantum-button-dot {
  width: 6px;
  height: 6px;

  border-radius: 50%;

  background: #facc15;

  box-shadow:
    0 0 5px #facc15,
    0 0 12px rgba(250, 204, 21, 0.7);

  animation: quantumButtonPulse 2s ease-in-out infinite;
}

.quantum-arrow {
  font-size: 0.9rem;
  opacity: 0.75;

  transition:
    transform 0.25s ease,
    opacity 0.25s ease;
}

.quantum-docs-button:hover .quantum-arrow {
  transform: translateX(3px);
  opacity: 1;
}

@keyframes quantumButtonPulse {
  0%, 100% {
    opacity: 0.45;
    transform: scale(0.85);
  }

  50% {
    opacity: 1;
    transform: scale(1);
  }
}

@media (max-width: 640px) {
  .quantum-docs-button {
    font-size: 0.72rem;
    padding: 0.42rem 0.75rem;
  }
}
</style>