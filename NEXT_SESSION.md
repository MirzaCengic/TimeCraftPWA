# Next Session Starting Point

## Immediate Action Plan

### 🎯 **Start Here Tomorrow**: Priority 1 Implementation

**Goal**: Implement two-mode interface (Setup/Performance toggle)

### **Step 1: Add Mode Toggle Button** (15 mins)
```html
<!-- Add to playback panel after line 401 -->
<div style="text-align: center; margin-bottom: 20px;">
    <button onclick="toggleMode()" id="modeToggle" 
            style="background: #9C27B0; padding: 10px 20px; font-size: 16px;">
        📱 Performance Mode
    </button>
</div>
```

### **Step 2: Add CSS for Performance Mode** (20 mins)
```css
/* Add after line 278 */
.performance-mode .section-builder-panel,
.performance-mode .arrangement-builder-panel {
    display: none;
}

.performance-mode .metronome-flash {
    width: 300px !important;
    height: 300px !important;
}

.performance-mode .status {
    font-size: 1.5em;
}

.performance-mode .status-value {
    font-size: 3rem;
}
```

### **Step 3: Add JavaScript Toggle Function** (15 mins)
```javascript
// Add after line 1546 in script section
let isPerformanceMode = false;

function toggleMode() {
    isPerformanceMode = !isPerformanceMode;
    const body = document.body;
    const button = document.getElementById('modeToggle');
    
    if (isPerformanceMode) {
        body.classList.add('performance-mode');
        button.textContent = '⚙️ Setup Mode';
        button.style.background = '#4CAF50';
    } else {
        body.classList.remove('performance-mode');
        button.textContent = '📱 Performance Mode';
        button.style.background = '#9C27B0';
    }
    
    updateVisualMetronome();
}
```

### **Expected Outcome** (50 mins total)
- Toggle button switches between modes
- Performance mode hides section builder and arrangement panels
- Metronome enlarges to 300px in performance mode
- Status text becomes larger and more prominent

---

## **Priority 1 Complete Checklist**

- [ ] **Two-mode interface** - Setup/Performance toggle
- [ ] **Larger metronome** - 300px+ with better visual hierarchy  
- [ ] **Mobile touch targets** - 44px minimum sizing
- [ ] **Quick section templates** - Reduce creation friction

## **Files to Focus On**

1. **metronome_prototype.html** - Main implementation file
   - Lines 400-465: Playback panel (add mode toggle)
   - Lines 7-278: CSS styles (add performance mode styles)
   - Lines 467+: JavaScript (add toggle function)

2. **UI_IMPROVEMENTS.md** - Reference for specific requirements
3. **CLAUDE.md** - Technical context when needed

## **Quick Commands for Fresh Session**

```bash
# Check current state
git status
git log --oneline -3

# Open main file for editing
# (File: metronome_prototype.html)

# Test changes
# Open in browser and test mode toggle
```

## **Success Criteria for Tomorrow**

1. **Mode toggle works**: Button switches interface modes
2. **Performance mode**: Metronome prominent, sections hidden
3. **Setup mode**: Full interface for editing
4. **No regressions**: Audio timing still works perfectly

## **Context Notes**

- Project: Professional section-based metronome PWA
- Current state: Functional prototype with audio timing perfected
- Next phase: UI/UX improvements for mobile and professional use
- Target: Intermediate/advanced musicians needing complex practice tools

## **If You Have Extra Time**

Move to Priority 1, Item 2: Implement larger metronome visual improvements
- Add subdivision dots around perimeter
- Enhance beat flash duration and colors
- Improve visual hierarchy of status information