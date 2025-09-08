# Super Field - Option

A single-select options component for Budibase applications with dropdown, input select, and radio button interfaces for choosing from predefined options.

## 🚀 Features

### Single Selection

- **Dropdown Select**: Traditional dropdown menu for option selection
- **Input Select**: Searchable input with dropdown options
- **Radio Buttons**: Visual radio button selection interface
- **Field Binding**: Dedicated options field with single value
- **Default Values**: Pre-selected default options

### Data Sources

- **Schema Options**: Options from data schema definitions
- **Data Queries**: Dynamic options from data sources with filtering
- **Custom Options**: Manually defined option lists
- **Real-time Updates**: Dynamic option loading and updates
- **Sorting & Filtering**: Configurable option ordering and limits

### User Experience

- **Intuitive Controls**: Familiar selection interfaces
- **Search & Filter**: Quick option finding in large lists
- **Event Handling**: On change events with selection context
- **Validation**: Required field and selection validation
- **Help Text**: Guidance for option selection

### Layout Options

- **Direction Control**: Column or row layout for radio buttons
- **Responsive Design**: Adapts to different screen sizes
- **Accessibility**: Full keyboard navigation and screen reader support
- **Visual Feedback**: Clear selection states and hover effects

### Advanced Features

- **Data Integration**: Direct connection to Budibase data sources
- **Conditional Logic**: Dynamic options based on other fields
- **Performance**: Efficient handling of large option sets
- **Integration**: Full Budibase automation integration

### Styling & Layout

- **Flexible Positioning**: Label placement options
- **Field Modes**: Form input or inline editing
- **Size Configuration**: Adjustable component width
- **Theme Integration**: Consistent with Budibase design

## 📝 Usage Instructions

### Basic Setup

1. Add the Super Field - Option component to your form
2. Bind to an options field in your data source
3. Choose control type (select, input select, or radio)
4. Configure options source and display settings

### Advanced Configuration

- **Data Binding**: Connect to data sources with filtering
- **Control Types**: Select appropriate interface for use case
- **Validation**: Set required field validation
- **Events**: Attach actions to option selection changes

### Common Use Cases

- **Category Selection**: Single category or type selection
- **Status Updates**: Status or priority level selection
- **Preference Settings**: User preference or configuration options
- **Survey Questions**: Single-choice survey responses
- **Data Classification**: Classification or tagging selections

## 🔧 Configuration Options

| Setting        | Type       | Description                         |
| -------------- | ---------- | ----------------------------------- |
| Field          | Options    | Single-select options field binding |
| Label          | String     | Display label text                  |
| Placeholder    | String     | Selection guidance text             |
| Default Value  | String     | Pre-selected option                 |
| Help Text      | String     | Help/instruction text               |
| Validation     | Rules      | Selection validation rules          |
| Options Source | Select     | Schema/Data/Custom options          |
| Control Type   | Select     | Select/Input/Radio interface        |
| Direction      | Select     | Column/Row layout for radio         |
| Data Source    | DataSource | Data source for dynamic options     |
| Filter         | Filter     | Option filtering configuration      |
| Sort Column    | Field      | Column for option sorting           |
| Sort Order     | Select     | Ascending/Descending sort           |
| Limit          | Number     | Maximum options to display          |
| Value Column   | Field      | Column for option values            |
| Label Column   | Field      | Column for option labels            |
| Custom Options | Options    | Manually defined options            |

## 📋 Events

### On Change

Triggered when an option is selected.

**Context:**

- `value`: Selected option value
- `field`: The bound field information

## 🎨 Styling

The component supports Budibase's styling system with:

- **Selection Interface**: Clean option display and selection
- **Radio Styling**: Customizable radio button appearance
- **Dropdown Design**: Professional dropdown menu styling
- **Responsive**: Mobile-optimized selection interfaces

## 🔍 Best Practices

- Choose control type based on number of options
- Use input select for large option lists
- Set appropriate data limits for performance
- Provide clear option labels and descriptions
- Implement validation for required selections
- Test selection interaction on various devices
