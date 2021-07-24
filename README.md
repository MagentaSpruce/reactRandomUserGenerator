# reactRandomUserGenerator
This React project utilizes an API to fetch and render random user data to a card. 

This project utilizes an external API to fetch the data to be displayed: "https://randomuser.me/api/";

A general walkthrough of the React code is given below:


After initial file directory setup, state values are setup.
```React
function App() {
  const [loading, setLoading] = useState(true);
  const [person, setPerson] = useState(null);
  const [title, setTitle] = useState("name");
  const [value, setValue] = useState("random person");
```

Next create a function to be called to display information whenever an icon is hovered over.
```React
  const handleValue = (e) => {
    console.log(e.target);
  };
```

After the initial setup is complete the return is worked on next. Data label is used to connect the button with the state value.
```React
    <main>
      <div className="block bcg-black"></div>
      <div className="block">
        <div className="container">
          <img
            src={(person && person.image) || defaultImage}
            alt="random user"
            className="user-img"
          />
          <p className="user-title">my {title} is </p>
          <p className="user-value">{value}</p>
          <div className="values-list">
            <button
              className="icon"
              data-label="name"
              onMouseOver={handleValue}
            >
              <FaUser />
            </button>
          </div>
        </div>
      </div>
    </main>
```  

Now rest of icons are added.
```React
          <div className="values-list">
            <button
              className="icon"
              data-label="name"
              onMouseOver={handleValue}
            >
              <FaUser />
            </button>
            <button
              className="icon"
              data-label="email"
              onMouseOver={handleValue}
            >
              <FaEnvelopeOpen />
            </button>
            <button className="icon" data-label="age" onMouseOver={handleValue}>
              <FaCalendarTimes />
            </button>
            <button
              className="icon"
              data-label="street"
              onMouseOver={handleValue}
            >
              <FaMap />
            </button>
            <button
              className="icon"
              data-label="phone"
              onMouseOver={handleValue}
            >
              <FaPhone />
            </button>
            <button
              className="icon"
              data-label="password"
              onMouseOver={handleValue}
            >
              <FaLock />
            </button>
          </div>
```

Now add the get new random user button.
```React
          <button className="btn" type="button">
            {loading ? "loading..." : "random user"}
          </button>
```

Hovering over the icons should display their correct data-label. Next start integrating data from the API by setting up the getPerson function to fetch the user data using destructuring.
```React
  const getPerson = async () => {
    const response = await fetch(url);
    const data = await response.json();
    const person = data.results[0];
    const { phone, email } = person;
    const { large: image } = person.picture;
    const { password } = person.login;
    const { first, last } = person.name;
    const { age } = person.dob;
    const { number, name } = person.location.street;
```

Now the function to create a new person from the fetched data is created.
```React
    const newPerson = {
      image,
      phone,
      email,
      password,
      age,
      street: `${number} ${name}`,
      name: `${first} ${last}`,
    };
    setPerson(newPerson);
    setLoading(false);
  };
```

Now the name is rendered.
```React
    setPerson(newPerson);
    setLoading(false);
    setTitle("name");
    setValue(newPerson.name);
```

Next the random user button functionality is added.
```React
          <button className="btn" type="button" onClick={getPerson}>
            {loading ? "loading..." : "random user"}
          </button>
```

Next the changing of display based on button hovering is enabled by way of the handleValue function using the data-label which competes during hover with the svg icon. This must first be taken care of to ensure the button is hovered not the svg.
```React
  const handleValue = (e) => {
    if (e.target.classList.contains("icon")) {
      const newValue = e.target.dataset.label;
      console.log(newValue);
    }
  };
```

Lastly the new values are displayed during hover.
```React
  const handleValue = (e) => {
    if (e.target.classList.contains("icon")) {
      const newValue = e.target.dataset.label;
      setTitle(newValue);
      setValue(person[newValue]);
    }
  };
```

***End walkthrough
