# ImGui-Sources

# Particles (1) (Put the following code in imgui.h)
```c++
IMGUI_API void        Particles(ImDrawList* d, ImVec2 b);

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

# Particles (2) (Put the following code in your memu)
```c++
ImDrawList* draw;
draw = ImGui::GetWindowDrawList();
ImVec2 screenSize = ImGui::GetIO().DisplaySize;
ImGui::Particles(draw, screenSize);
```
