### HarmonyOS PC/2in1 Development Treasure Guide: Official Case Battle Analysis  

Hello everyone! Recently, while diving into HarmonyOS PC/2in1 application development, I discovered a trove of ultra-practical cases hidden in the official documentation! These cases are like "hidden levels," helping you avoid 80% of development pitfalls. Today, I’ve sorted out these treasures and will guide you through hands-on practice with code—you’ll definitely exclaim "so awesome" after reading!  


### 🌟 I. Layout Design: Maximizing Every Inch of the Large Screen  
**Official core concept**: Use responsive layouts + breakpoint mechanisms for dynamic adaptation to different screen sizes. **Critical**: Add `deviceTypes: ["2in1"]` in `module.json5`, or PC features won’t take effect!  

#### 1. Side Hierarchical Navigation Bar  
**Scenario**: When window width ≥1440vp, bottom navigation becomes a sidebar (inspired by desktop VS Code).  
**Core code**:  
```typescript
// Use SideBarContainer component  
SideBarContainer() {  
  Column() { /* Sidebar area */ }  
  Column() { /* Main content area */ }  
}  
.showSideBar(this.currentBreakpoint === 'xl') // Show sidebar at xl breakpoint  
.sideBarWidth(300) // Initial width  
```  
**Case analysis**:  
● Control sidebar position with `sideBarPosition` (left/right).  
● Listen to `onAreaChange` event for drag-and-resize.  

#### 2. Repeating Layouts: Lists/Waterflows/Grids  
**Pain point**: Single column on small screens → multi-column on large screens to enhance information density.  
**Key attributes**:  
● List: `List().lanes(3)` (3 columns on large screens, 1 column by default on small screens)  
● Waterflow: `WaterFlow().columnsTemplate('1fr 1fr 2fr')` (third column doubles width)  
● Grid: `Grid().columnsTemplate('1fr 1fr 1fr')`  

```typescript
// Waterflow responsive example  
WaterFlow() {  
  // Content items...  
}  
.columnsTemplate(  
  currentBreakpoint === 'xl' ? '1fr 1fr 1fr' : '1fr 1fr' // 3 columns at XL breakpoint  
)
```  

#### 3. Large-Screen Optimization for Carousels  
**God-level operation**: Display multiple items + edge exposure on large screens.  
```typescript
Swiper() {  
  // Carousel items...  
}  
.displayCount(3) // Show 3 items on large screens  
.itemSpace(20)   // Image spacing  
.prevMargin(30)  // Expose edge of previous item  
.nextMargin(30)  // Expose edge of next item  
.aspectRatio(1.78) // Lock aspect ratio to prevent distortion  
```  


### 🖥️ II. Window Adaptation: Free Window Tricks  
#### 1. Free Window & Immersive Mode  
**Mandatory configuration** (in `module.json5`):  
```json
"abilities": [{  
  "supportWindowMode": ["fullscreen", "split"]  
}]
```  
**Key capabilities**:  
● Listen to window size changes via `window.on('windowSizeChange')`.  
● Hide title bar: `windowClass.setWindowSystemBarEnable([])`.  

#### 2. Column Layout (Similar to iPad Multitasking)  
**Combination approach**: `Navigation + SideBarContainer`  
```typescript
Navigation() {  
  SideBarContainer() {  
    // Left column  
    Column() {...}  
    // Right column  
    Column() {...}  
  }  
}
```  


### 🖱️ III. Keyboard-Mouse Interaction: Making PC Experience More Native  
#### 1. Mouse Hover Effects  
**Official requirement**: All interactive components must support this!  
```typescript
Button('Submit')  
.onHover((isHover) => {  
  if (isHover) this.bgColor = '#e1e1e1' // Hover color change  
})
```  

#### 2. Keyboard Shortcuts  
**Listen for global key presses**:  
```typescript
import { KeyEvent } from '@ohos.multimodalInput'  

onKeyEvent(event: KeyEvent) {  
  if (event.keyCode === 4097 && event.ctrlKey) { // Ctrl+S  
    this.saveData()  
  }  
}
```  
**Common key codes**:  
● Enter: 1001  
● Ctrl: 4097  
● Alt: 4098  

#### 3. Focus Navigation  
**Tab key focus switching**:  
```typescript
TextInput()  
.tabIndex(1) // Set Tab order  
.onFocus(() => this.showFocusStyle())
```  


### 💡 IV. Pitfall Prevention Guide (Hard-Earned Lessons!)  
1. **Camera invocation**: PCs may have multiple cameras—use `getCameraDevices()` for dynamic acquisition.  
2. **Resolution traps**: Prepare three sets of image resources: hdpi/xhdpi/xxhdpi.  
3. **Prevent window stretch crashes**: Use `vp` units for all sizes; never hardcode `px`!  
4. **Multi-device testing commands**:  
```bash
hdc shell aa start -a EntryAbility -b com.demo.app -d tablet  
hdc shell aa start -a EntryAbility -b com.demo.app -d 2in1
```  


### Conclusion  
HarmonyOS PC/2in1 development documentation is like a goldmine, though slightly hidden 😅. All cases in this article come from the official *One-Many Development Practice* documentation—highly recommended for parallel reading!  

**Hands-on challenge**: Use `SideBarContainer+WaterFlow` to replicate Bilibili’s PC homepage. Three brothers who share their work in the comments will randomly win HarmonyOS custom peripherals 🎁!  

Got questions? Fire away 👇 What hidden HarmonyOS trick should I uncover next? See you in the comments!
