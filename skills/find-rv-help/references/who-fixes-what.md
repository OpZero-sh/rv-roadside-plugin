# Who fixes what on an RV

General orientation for routing a problem to the right kind of shop. The maker's documentation and the connector's `rv_guide` take precedence over anything here.

## An RV is several products in one

- **Chassis.** On a motorhome, the frame, engine, transmission, steering, suspension and brakes come from a chassis maker, which is often a different company from the RV brand on the side. The chassis maker's name is in the owner's documents and on the chassis identification label.
- **House, also called the coach.** The living space built by the RV maker: walls, roof, slide rooms, plumbing, 12 volt and 120 volt house wiring, cabinetry.
- **Appliances and components.** The refrigerator, water heater, furnace, air conditioner, generator, awning, leveling system and similar parts each come from their own maker and carry their own model and serial number plate.
- **Running gear on a towable.** Axles, hubs, bearings, brakes, suspension and tires on a travel trailer or fifth wheel. The tow vehicle is a separate vehicle with its own service network.

Warranty coverage usually follows the same split: the chassis maker, the RV maker and each component maker cover their own parts.

## Routing table

| Problem area | Typical signs | Kind of shop | `need` for `find_rv_repair` |
| --- | --- | --- | --- |
| Motorhome engine, transmission, steering, brakes, suspension, chassis electrical | Warning lights, no start, overheating, air brake faults, leaks under the drivetrain | A truck or chassis service shop that works on that chassis make and can take the vehicle's size | `heavy_truck` |
| Van or small motorhome on a van or pickup chassis | Same signs on a smaller vehicle | Often the chassis brand's dealer or a shop with a bay tall enough. Confirm height and weight limits | `heavy_truck` or `rv_service`, then ask |
| House plumbing, house electrical, slide rooms, roof, leaks, leveling | Water where it should not be, dead outlets, a slide that will not move | RV dealer service department, independent RV shop, or a mobile RV technician | `rv_service` |
| Appliances: refrigerator, water heater, furnace, air conditioner | Not cooling, not heating, will not light | RV shop or mobile RV technician. Some appliance makers list authorized service centers | `rv_service` |
| Generator | Will not start, starts and stops, no output | RV shop, or a service dealer for the generator's maker | `rv_service` |
| Trailer axles, bearings, brakes, suspension | Hot hub, grinding, uneven tire wear, broken spring | Trailer or RV shop. Some truck shops do this work | `rv_service`, or `heavy_truck` if none |
| Tires | Flat, blowout, bulge, tread separation, old tires | Tire shop or mobile tire service that handles the size and load range | `tires` |
| Cannot move under its own power | Any of the above with the rig disabled | Towing or roadside service able to handle the rig's weight and length | `towing` |
| Tow vehicle problems | The truck or SUV, not the trailer | Ordinary auto or truck repair. Outside this connector's focus | none |

The `need` values are the ones this plugin documents. The tool's own schema is the authority on which values exist.

## Size changes everything

- A large motorhome cannot be lifted or towed by ordinary equipment. Towing needs a heavy wrecker or a trailer rated for the weight. Always give the tow company the length and weight.
- Many auto shops cannot fit an RV through the door or on a lift. Ask about maximum height, length and weight before driving there.
- Mobile technicians come to the rig, which helps when it is in a campground, but they usually do house and appliance work, not chassis work.

## When it is not clear

- If the user cannot tell whether a failure is chassis or house, ask what stopped working and whether the vehicle still drives normally. Problems that affect driving are chassis. Problems that affect living in it are house or appliance.
- For a component with its own data plate, the maker named on the plate is the place to start for manuals and authorized service. `rv_guide` with `section: "plates_and_parts"` covers where plates are found.
