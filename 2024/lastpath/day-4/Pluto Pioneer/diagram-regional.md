Diagram regional


```mermaid
graph TD
    Control["Admin / Control\n192.168.10.173/24"] --> Data["Data\n192.168.10.68/24\n"]
    Control --> Internal["Internal\n192.168.10.71/24\n\nAtom/SLiMS :8080"]
    Data --> Public["Public\n192.168.10.78/24 (wlan0)\n"]
    Public --> Client["Client"]
    Client -->|akses web| Public
