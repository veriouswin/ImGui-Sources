# ImGui-Sources

<br>

## Particles

<details>
<summary>Click here to see the code.</summary>

<br>
    
(1) Paste this into your imgui.h file
    
```c++
IMGUI_API void        Particles(ImDrawList* d, ImVec2 b);

#include <vector>
int N = 600;
int lineMaxDist = 1400;
ImColor lineCol = { 255, 255, 255, 60 };
float lineThickness = 1.0f;

void setupPoints(std::vector<std::pair<ImVec2, ImVec2>>& n) {
    ImVec2 screenSize(ImGui::GetIO().DisplaySize);
    for (auto& p : n)
        p.second = p.first = ImVec2(rand() % (int)screenSize.x, rand() % (int)screenSize.y);
}

float length(ImVec2 x) { return x.x * x.x + x.y * x.y; }

void ImGui::Particles(ImDrawList* d, ImVec2 b)
{
    static std::vector<std::pair<ImVec2, ImVec2>> points(N);
    static auto once = (setupPoints(points), true);
    float Dist;
    for (auto& p : points) {
        Dist = sqrt(length(p.first - p.second));
        if (Dist > 0) p.first += (p.second - p.first) / Dist;
        if (Dist < 4) p.second = ImVec2(rand() % (int)b.x, rand() % (int)b.y);
    }
    for (int i = 0; i < N; i++) {
        for (int j = i + 1; j < N; j++) {
            Dist = length(points[i].first - points[j].first);
            if (Dist < lineMaxDist) d->AddLine(points[i].first, points[j].first, lineCol, lineThickness);
        }
    }
}
```
<br>
    
(2) Paste this into your menu
```c++
ImDrawList* draw;
draw = ImGui::GetWindowDrawList();
ImVec2 screenSize = ImGui::GetIO().DisplaySize;
ImGui::Particles(draw, screenSize);
```

</details>
    
<br>

## Progress Bar

<details>
<summary>Click here to see the code.</summary>

<br>

(1) Paste this on top of your code
   
```c++
bool animate = false;
```
<br>

(2) Paste this below (ex.) your button
   
```c++
static float progress = 0.0f;
static float progress_dir = 1.0f;
if (animate)
{
    progress += progress_dir * 0.4f * ImGui::GetIO().DeltaTime;
}
if (animate)
{
    ImGui::SetCursorPos({ 100, 86 });
    ImGui::SetNextItemWidth(150);
    ImGui::ProgressBar(progress, ImVec2(0.0f, 30.0f));
}
```
<br>
    
(3) Paste this in your buttons 'if' statement
```c++
animate = true;
```

</details>
