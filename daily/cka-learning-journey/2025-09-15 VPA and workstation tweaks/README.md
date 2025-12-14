
## Activities Covered

- **Kubernetes – Vertical Pod Autoscaler (VPA)**
    
    - Looked into **resource policy** and **container policy** for fine-tuned control.        
    - Policies allow restricting which containers within a pod can be resized and to what extent, ensuring better workload predictability.

---
## Open Source Solutions Explored

### Problem: External Mouse Rough Scrolling on Mac

- **Issue Observed:**
    
    - Scrolling with an external Bluetooth mouse is **very rough**, often jumping randomly backward or forward.
        
- **Root Cause (Mac Built Mechanism):**

    - **Touchpad:** Scrolls smoothly by **pixels**.        
    - **External Mouse:** Scrolls by **pointer**, leading to jarring scroll jumps.

### Solution Tried: **LinearMouse (Open Source)**

- **Installation:** Installed an app called **LinearMouse** to smooth out external mouse scrolling.
    
- **Effectiveness:**
    
    - Fixed the issue.        
    - Not as seamless as touchpad scrolling, but **far smoother and usable** compared to default behavior.
        
- **Concerns:**
    
    - **Security:** No signs of data sniffing.
    - **Performance:** Lightweight, requires running in the background all the time to remain effective, which is acceptable.
---
## Key Takeaways
 
- Kubernetes VPA provides container-level resource adjustment policies, which are crucial for controlled autoscaling.
    
- Open-source desktop utilities (like LinearMouse) effectively solve hardware/software integration gaps, especially in macOS environments.
