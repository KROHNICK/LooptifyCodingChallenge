//# LooptifyCodingChallenge
//Hello Jim, this is my submission of the coding challenge.

import { useState } from "react";
import "./styles.css";

/*
Challenge:
Created array of checkboxes, some options has nested array
render label, and check box

track what user have selected, user may select more than 1
nested array of checkboxes should be selectable when user checks 'Various'

Acceptance:
Render all flavors, render addons nested in the correct flavor  with checkboxes
User should be able to select multiple checkboxes
User should be allowed to select addons, if available
Every selection should console log an array of currently selected all flavor, 
and flavor with addon

* style does not matter
* you do not need to track historical selection

[
  {
    id: 0,
    label: 'Strawberry Icecream',
    type: 'FLAVOR'
  },
  {
    id: 1,
    label: 'Vanilla Icecream',
    type: 'FLAVOR'
  },
  {
    id: 2,
    label: 'Chocolate Icecream',
    type: 'FLAVOR'
  },
  {
    id: 3,
    label: 'Various Flavors',
    type: 'FLAVOR',
    data: [
      {
        id: 0,
        label: 'Oreo Cookies',
        type: 'ADDON'
      },
      {
        id: 1,
        label: 'Vanilla Waffers',
        type: 'ADDON'
      }
    ]
  }
]

*/

export default function App() {
  const [cart, setCart] = useState([]);
  const [isCurChecked, setIsCurChecked] = useState(true);

  const array = [
    {
      id: 0,
      label: "Strawberry Icecream",
      type: "FLAVOR"
    },
    {
      id: 1,
      label: "Vanilla Icecream",
      type: "FLAVOR"
    },
    {
      id: 2,
      label: "Chocolate Icecream",
      type: "FLAVOR"
    },
    {
      id: 3,
      label: "Various Flavors",
      type: "FLAVOR",
      data: [
        {
          id: 0,
          label: "Oreo Cookies",
          type: "ADDON"
        },
        {
          id: 1,
          label: "Vanilla Waffers",
          type: "ADDON"
        }
      ]
    }
  ];

  const isChecked = (e) => {
    let tempCart = [...cart];
    if (array[e.target.value].label === "Various Flavors") {
      setIsCurChecked(true);
    }
    if (e.target.checked) {
      if (array[e.target.value].label !== "Various Flavors") {
        tempCart.push(array[e.target.value].label);
        setCart(tempCart);
      }
      if (array[e.target.value].label === "Various Flavors") {
        setIsCurChecked(false);
      }
      console.log(tempCart);
    }
    if (e.target.checked === false) {
      let index = tempCart.indexOf(array[e.target.value].label);
      if (array[e.target.value].label === "Various Flavors") {
        console.log(tempCart);
      } else if (
        array[e.target.value].label !== "Various Flavors" ||
        array[e.target.value].label !== "Oreo Cookies" ||
        array[e.target.value].label !== "Vanilla Waffers"
      ) {
        tempCart.splice(index, 1);
        setCart(tempCart);
      }
      if (array[e.target.value].label === "Various Flavors") {
        for (let i = 0; i < tempCart.length; i++) {
          if (tempCart[i] === "Oreo Cookies") {
            tempCart.splice(i, 1);
          }
          if (tempCart[i] === "Vanilla Waffers") {
            tempCart.splice(i, 1);
          }
        }
        unCheck();
        setCart(tempCart);
      }
      console.log(tempCart);
    }
  };

  const addOnIsChecked = (e, icecreamOption) => {
    let tempCart = [...cart];
    if (e.target.checked) {
      tempCart.push(array[icecreamOption].data[e.target.value].label);
      setCart(tempCart);
      console.log(tempCart);
    }
    if (e.target.checked === false) {
      let index = tempCart.indexOf(
        array[icecreamOption].data[e.target.value].label
      );
      tempCart.splice(index, 1);
      setCart(tempCart);
      console.log(tempCart);
    }
  };

  const unCheck = () => {
    let tempCart = [...cart];
    let chk = document.getElementsByClassName("addon");
    tempCart.forEach(() => {
      if (chk) {
        console.log(chk.checked);
      }
    });
    setCart(tempCart);
  };

  return (
    <div className="App">
      <h1>Hello CodeSandbox</h1>
      <h2>Start editing to see some magic happen!</h2>
      <div style={{ display: "flex", justifyContent: "center" }}>
        {array.map((icecreamOption) => {
          return (
            <div style={{ margin: "10px" }} key={icecreamOption.id}>
              <p>{icecreamOption.label}</p>
              <input
                key={icecreamOption.id}
                value={icecreamOption.id}
                type="checkbox"
                onClick={(e) => isChecked(e)}
              />
              {icecreamOption?.data &&
                icecreamOption.data.map((addOn) => {
                  return (
                    <div style={{ display: "flex" }} key={addOn.id}>
                      <p>{addOn.label}</p>
                      <input
                        className="addon"
                        key={addOn.id}
                        value={addOn.id}
                        type="checkbox"
                        onClick={(e) => addOnIsChecked(e, icecreamOption.id)}
                        disabled={isCurChecked}
                        checked={!isCurChecked ? null : false}
                      />
                    </div>
                  );
                })}
            </div>
          );
        })}
      </div>
    </div>
  );
}
