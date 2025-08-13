const slots = {
  "!first": { id: "firstSlot", sound: "firstSound" },
  "!second": { id: "secondSlot", sound: "secondSound" },
  "!third": { id: "thirdSlot", sound: "thirdSound" }
};

const claimed = {};

window.addEventListener('onEventReceived', function (obj) {
  if (obj.detail.listener !== "message") return;

  const msg = obj.detail.event.data.text.toLowerCase();
  const user = obj.detail.event.data.displayName;

  if (msg === "!clear") {
    Object.values(slots).forEach(({ id }) => {
      const el = document.getElementById(id);
      el.textContent = "—";
    });
    Object.keys(claimed).forEach(key => delete claimed[key]);
    return;
  }

  if (slots[msg] && !claimed[msg]) {
    const { id, sound } = slots[msg];
    const el = document.getElementById(id);
    el.textContent = user;
    el.classList.add("pulse");
    setTimeout(() => el.classList.remove("pulse"), 1000);

    const audio = document.getElementById(sound);
    if (audio) audio.play();

    claimed[msg] = true;
  }
});
