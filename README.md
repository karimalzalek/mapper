# Mapper

**Mapper** is a web-based tool built with Vue.js and Mapbox that allows users to add multiple locations on a map. Users can search for locations by name or enter geographic coordinates (latitude and longitude) to pinpoint locations directly. Once locations are added, Mapper calculates and displays the shortest route between them.

## Features

- **Location Search**: Search for a place by name and add it to the map.
- **Geocode Entry**: Enter latitude and longitude directly to mark a location.
- **Interactive Map**: Adjust the map view and refine location placement by moving map markers.
- **Multiple Stops**: Add multiple locations as stops on the map.
- **Shortest Route Calculation**: Automatically calculate the nearest route between all added stops

## Use Cases

This tool can be helpful in various scenarios:

- **Vacation Planning**: Organize stops for your travel itinerary, ensuring you visit the top spots efficiently.
- **Customer Visits**: Map out the best route to visit multiple clients in a single trip.
- **Delivery and Logistics**: Plan delivery routes by identifying optimal routes between multiple drop-off points.
- **Event Planning**: For planners or coordinators who need to visit multiple locations, this tool helps in creating a streamlined route.

## Screenshots

To be added

## Tech Stack

- **Vue.js**: For building a responsive and user-friendly interface.
- **Mapbox**: For interactive map features and geolocation.

## Installation and Setup

1. **Clone the repository**:
    ```bash
    git clone https://github.com/your-username/mapper.git
    cd mapper
    ```

2. **Install dependencies**:
    ```bash
    npm install
    ```

3. **Configure Mapbox API Key**:
    - **Create a Mapbox Account**: Go to [Mapbox](https://account.mapbox.com/auth/signup/) to sign up or log in.
    - **Create an Access Token**:
      1. Once logged in, go to the **Tokens** page at [https://account.mapbox.com/access-tokens/](https://account.mapbox.com/access-tokens/).
      2. Click **Create a token** and customize the settings if needed.
      3. Copy the generated token.
    - **Set Up Environment File**:
      1. In the root directory, create a `.env` file.
      2. Add your Mapbox API key as follows:
         ```plaintext
         VUE_APP_MAPBOX_ACCESS_TOKEN=your_mapbox_access_token_here
         ```

4. **Run the development server**:
    ```bash
    npm run serve
    ```

    This should start the application on `http://localhost:8080`.

## Usage

1. **Add a Location**:
    - Use the search bar to find a location by name or enter latitude and longitude to add a new location directly to the map.
    - Each location will be marked with a pin.

2. **Adjust Location**:
    - Click and drag markers to refine the placement if needed.

3. **Create Shortest Route**:
    - After adding multiple stops, press the "Calculate Route" button to automatically generate the shortest path between all added locations.

## Future Enhancements

- **Saving Routes**: Allow users to save and load routes.
- **Export Route Data**: Export route information for offline use.
- **Integration with Other Mapping Services**: Extend compatibility with additional mapping APIs.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request if you have suggestions or improvements.

## License

This project is licensed under the **GNU General Public License**.

---

Happy Mapping with Mapper!
