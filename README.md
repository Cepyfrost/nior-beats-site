// React app for NIOR Beats
// Features: Owner-only upload, beat previews (no download), Paywall-ready

import { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Upload, Lock } from "lucide-react";

const BEATS = [
  { title: "Afro Heat", url: "/beats/afro_heat_preview.mp3" },
  { title: "Dark Trap", url: "/beats/dark_trap_preview.mp3" },
  { title: "Lagos Night", url: "/beats/lagos_night_preview.mp3" },
];

const OWNER_EMAIL = "cprogress590@gmail.com"; // Replace with your real email

export default function NiorBeats() {
  const [email, setEmail] = useState("");
  const [isOwner, setIsOwner] = useState(false);

  const handleLogin = () => {
    if (email.trim().toLowerCase() === OWNER_EMAIL.toLowerCase()) {
      setIsOwner(true);
    } else {
      alert("Access denied: Only NIOR Beats can upload.");
    }
  };

  return (
    <div className="min-h-screen bg-black text-white p-6">
      <h1 className="text-4xl font-bold text-center mb-6">🎧 NIOR Beats</h1>

      {/* Upload area (owner only) */}
      <div className="mb-8">
        {!isOwner ? (
          <div className="flex gap-2 items-center max-w-sm mx-auto">
            <Input
              placeholder="Enter admin email"
              value={email}
              onChange={(e) => setEmail(e.target.value)}
              className="text-black"
            />
            <Button onClick={handleLogin}>
              <Upload className="mr-2 w-4 h-4" /> Admin Login
            </Button>
          </div>
        ) : (
          <div className="text-center">✅ Logged in as NIOR Beats — Upload UI Coming Soon</div>
        )}
      </div>

      {/* Beat previews */}
      <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
        {BEATS.map((beat, index) => (
          <Card key={index} className="bg-neutral-900 border border-neutral-700">
            <CardContent className="p-4">
              <h2 className="text-xl font-semibold mb-2">{beat.title}</h2>
              <audio
                controls
                src={beat.url}
                controlsList="nodownload"
                className="w-full"
                onContextMenu={(e) => e.preventDefault()}
              />
              <Button disabled className="mt-4 w-full">
                <Lock className="mr-2 w-4 h-4" /> Purchase to Download
              </Button>
            </CardContent>
          </Card>
        ))}
      </div>

      {/* Anti-inspect / hardening logic (basic) */}
      <script>
        {`
          document.addEventListener('contextmenu', event => event.preventDefault());
          document.onkeydown = function(e) {
            if (e.keyCode === 123 || (e.ctrlKey && e.shiftKey && e.keyCode === 73)) {
              return false;
            }
          }
        `}
      </script>
    </div>
  );
}
