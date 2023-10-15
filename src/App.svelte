<script>
  import Inputmask from "inputmask";
  import creditCardType from "credit-card-type";

  let numberInput,
    nameInput,
    expireInput,
    cvvInput,
    backface = false;

  let card = {
    number: "",
    name: "",
    expiry: "",
    cvv: "",
  };

  $: if (numberInput && expireInput && cvvInput) {
    Inputmask({
      mask: "9999 9999 9999 9999",
      placeholder: " ",
    }).mask(numberInput);
    Inputmask({
      mask: "99/99",
      placeholder: " ",
    }).mask(expireInput);
    Inputmask({
      mask: "999",
      placeholder: " ",
    }).mask(cvvInput);
  }

  $: type = card.number ? creditCardType(card.number)?.[0]?.type : false;

  const UpperString = (str) => {
    var parcalar = str.split(" ");
    for (var i = 0; i < parcalar.length; i++) {
      var j = parcalar[i].charAt(0).toUpperCase();
      parcalar[i] = j + parcalar[i].substr(1).toLowerCase();
    }
    return parcalar.join(" ");
  };

  const create = () => {
    console.log(card);
  };
</script>

<main>
  <div class="credit-cart" id="credit-cart" class:flipped={backface}>
    <div class="front">
      <div class="start">
        <div class="chip">
          <img src="/images/chip.svg" alt="" />
        </div>
        <div class="brand">
          {#if type}
            <img src="/images/{type}.svg" alt="" />
          {/if}
        </div>
      </div>
      <div class="center">
        <div class="card-number">{card.number || "**** **** **** ****"}</div>
      </div>
      <div class="end">
        <div class="cart-name">
          <div class="key">Card Holder Name</div>
          <div class="value">{UpperString(card.name) || "***"}</div>
        </div>
        <div class="card-date">
          <div class="key">Expiry Date</div>
          <div class="value">{card.expiry || "***"}</div>
        </div>
      </div>
    </div>
    <div class="back">
      <div class="cvv">
        <div class="key">CVV</div>
        <em class="value">{card.cvv || "***"}</em>
      </div>
    </div>
  </div>
  <div class="credit-cart-settings">
    <h1>Kredi Kartı Ayarları</h1>
    <div class="form-group">
      <input
        type="text"
        id="card-number"
        placeholder="Kart Numarası"
        bind:value={card.number}
        bind:this={numberInput}
      />
    </div>
    <div class="form-group">
      <input
        type="text"
        id="card-name"
        placeholder="Kart Sahibi"
        bind:value={card.name}
        bind:this={nameInput}
      />
    </div>
    <div class="form-group">
      <input
        type="text"
        id="card-date"
        placeholder="Son Kullanma Tarihi"
        bind:value={card.expiry}
        bind:this={expireInput}
      />
    </div>
    <div class="form-group">
      <input
        type="text"
        id="card-cvv"
        placeholder="CVV"
        bind:value={card.cvv}
        bind:this={cvvInput}
        on:focus={() => (backface = true)}
        on:blur={() => (backface = false)}
      />
    </div>
    <div class="form-group">
      <label class="backface-handle">
        <input type="checkbox" bind:checked={backface} />
        Arka yüzünü {backface ? "gizle" : "göster"}
      </label>
    </div>
    <div class="form-group">
      <button type="button" on:click={create}>Oluştur</button>
    </div>
  </div>
</main>

<style lang="scss">
  main {
    padding: 5rem;
    .credit-cart {
      width: 350px;
      height: 220px;
      position: relative;
      perspective: 800px;
      .front,
      .back {
        width: inherit;
        height: inherit;
        box-sizing: border-box;
        background: linear-gradient(89.98deg, #885df5 0%, #6b7cfe 99.41%);
        border-radius: 15px;
        position: absolute;
        top: 0;
        left: 0;
        backface-visibility: hidden;
        transition: 1s all;
        padding: 31px 27px;
        display: flex;
        flex-direction: column;
      }
      .front {
        justify-content: space-between;
        box-sizing: border-box;
        .start {
          display: flex;
          align-items: center;
          justify-content: space-between;
        }
        .center {
          .card-number {
            font-size: 28px;
            color: #fff;
            margin-top: 20px;
            font-family: monospace;
            letter-spacing: -1.5px;
          }
        }
        .end {
          color: #fff;
          display: flex;
          align-items: center;
          justify-content: space-between;
          .key {
            font-size: 14px;
            font-weight: 500;
            opacity: 0.75;
            letter-spacing: -0.3px;
            margin-bottom: 5px;
          }
          .value {
            font-size: 18px;
            font-weight: 500;
          }
        }
      }
      .back {
        transform: rotateY(180deg);
        .cvv {
          background-color: #fff;
          border-radius: 7.5px;
          width: 100%;
          display: flex;
          padding: 1rem;
          box-sizing: border-box;
          gap: 7.5px;
          font-size: 1rem;
          color: #333;
          font-weight: 500;
          .key {
            margin-left: auto;
          }
          .value {
            font-weight: 600;
          }
        }
      }
      &.flipped {
        .front {
          transform: rotateY(-180deg);
        }
        .back {
          transform: rotateY(0deg);
        }
      }
    }
    .credit-cart-settings {
      margin-top: 1.5rem;
      max-width: 350px;
      h1 {
        font-size: 1.5rem;
        font-weight: 500;
        margin-bottom: 0.75rem;
      }
      .form-group {
        display: flex;
        flex-direction: column;
        margin-bottom: 5px;
        label {
          display: block;
          font-size: 1rem;
          color: #333;
          cursor: pointer;
        }
        input {
          border: 1px solid #ccc;
          width: 320px;
          padding: 0.5rem;
          border-radius: 0.25rem;
          margin: 5px 0;
          font-size: 1rem;
          color: #000;
        }
        .backface-handle {
          display: flex;
          align-items: center;
          gap: 5px;
          input[type="checkbox"] {
            all: unset;
            appearance: none;
            width: 17px;
            height: 17px;
            border-radius: 5px;
            border: 1px solid #ccc;
            display: inline-block;
            transition: 250ms all;
            &::before {
              content: "";
              width: 17px;
              height: 17px;
              display: block;
              background-color: #885df5;
              opacity: 1;
              background-image: url("/images/check.svg");
              background-size: 1rem;
              fill: #fff;
              color: #fff;
              opacity: 0;
            }
            &:checked::before {
              opacity: 1;
            }
          }
        }
        button {
          background-color: #1da1f2;
          max-width: 218px;
          width: 100%;
          height: 45px;
          border-radius: 4px;
          font-size: 16px;
          margin-top: 0.5rem;
          color: #fff;
          background: linear-gradient(89.98deg, #885df5 0%, #6b7cfe 99.41%);
          cursor: pointer;
          transition: 250ms all;
          &:hover {
            background: linear-gradient(89.98deg, #7d55e4 0%, #6372e9 99.41%);
          }
        }
      }
    }
  }
</style>
