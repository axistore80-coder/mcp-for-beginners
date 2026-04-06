# ការប្រើប្រាស់ម៉ាស៊ីនបម្រើកម្រិតខ្ពស់

មានម៉ាស៊ីនបម្រើពីរប្រភេទផ្សេងគ្នា ដែលបង្ហាញក្នុង MCP SDK គឺម៉ាស៊ីនបម្រើធម្មតារបស់អ្នក និងម៉ាស៊ីនបម្រើកម្រិតទាប។ ជាទូទៅ អ្នកនឹងប្រើម៉ាស៊ីនបម្រើធម្មតា ដើម្បីបន្ថែមមុខងារផ្សេងៗទៅ។ ប៉ុន្តែក្នុងករណីខ្លះ អ្នកចង់ពឹងផ្អែកលើម៉ាស៊ីនបម្រើកម្រិតទាប ដូចជា៖

- សំណង់ល្អប្រសើរ។ អាចបង្កើតសំណង់ស្អាតមួយដែលរួមបញ្ចូលម៉ាស៊ីនបម្រើធម្មតា និងម៉ាស៊ីនបម្រើកម្រិតទាបទាំងពីរ ប៉ុន្តែក៏អាចត្រូវបានចោទថាវាងាយស្រួលបន្តិចជាមួយម៉ាស៊ីនបម្រើកម្រិតទាប។
- ការជម្រុញមុខងារ។ មុខងារឧត្តមខ្លះអាចប្រើបានតែនៅក្នុងម៉ាស៊ីនបម្រើកម្រិតទាបប៉ុណ្ណោះ។ អ្នកនឹងឃើញវានៅក្នុងមជ្ឈទីភាយក្រោយពេលយើងបន្ថែមការទាញយក និងការបញ្ចេញមតិយោបល់។

## ម៉ាស៊ីនបម្រើធម្មតា និង ម៉ាស៊ីនបម្រើកម្រិតទាប

នេះគឺរូបរាងនៃការបង្កើតម៉ាស៊ីនបម្រើ MCP ដោយប្រើម៉ាស៊ីនបម្រើធម្មតា

**Python**

```python
mcp = FastMCP("Demo")

# បន្ថែមឧបករណ៍បូក
@mcp.tool()
def add(a: int, b: int) -> int:
    """Add two numbers"""
    return a + b
```

**TypeScript**

```typescript
const server = new McpServer({
  name: "demo-server",
  version: "1.0.0"
});

// បន្ថែមឧបករណ៍បូក
server.registerTool("add",
  {
    title: "Addition Tool",
    description: "Add two numbers",
    inputSchema: { a: z.number(), b: z.number() }
  },
  async ({ a, b }) => ({
    content: [{ type: "text", text: String(a + b) }]
  })
);
```

ចំណុចគឺ អ្នកបញ្ជាក់យ៉ាងច្បាស់នូវឧបករណ៍ ផ្នែកធនធាន ឬពន្លឺដែលអ្នកចង់ឲ្យម៉ាស៊ីនបម្រើមាន។ វាមិនមានកំហុសអ្វីនោះទេ។

### វិធីសាស្រ្តម៉ាស៊ីនបម្រើកម្រិតទាប

ប៉ុន្តែក្នុងពេលអ្នកប្រើវិធីសាស្រ្តម៉ាស៊ីនបម្រើកម្រិតទាប អ្នកត្រូវគិតខុសគ្នា។ មិនត្រូវចុះបញ្ជីឧបករណ៍នីមួយៗទេ តែអ្នកបង្កើតម្ចាស់ការសម្រាប់មុខងារ (ឧបករណ៍, ធនធាន ឬពន្លឺ) ពីរនាក់។ ឧទាហរណ៍ ដូចជា ឧបករណ៍មានមុខងារពីរនេះ៖

- បង្ហាញបញ្ជីឧបករណ៍ទាំងអស់។ មុខងារមួយដើម្បីចាត់ចែងនូវការបញ្ជីឧបករណ៍ទាំងអស់។
- ដំណើរការហៅឧបករណ៍ទាំងអស់។ ផងដែរ មានមុខងារមួយដែលដំណើរការការហៅឧបករណ៍។

វាហាក់ដូចជាការងារស្រួលតិចមែនទេ? ដូច្នេះជំនួសការចុះបញ្ជីឧបករណ៍ វាអ្នកគ្រាន់តែធានាថាឧបករណ៍ត្រូវបានបញ្ជីនៅពេលអ្នកបង្ហាញបញ្ជីឧបករណ៍ទាំងអស់ និងត្រូវបានហៅនៅពេលមានសំណើមកទាក់ទងនឹងការហៅឧបករណ៍។

ឱកាសមើលរបៀបកូដឥឡូវនេះ៖

**Python**

```python
@server.list_tools()
async def handle_list_tools() -> list[types.Tool]:
    """List available tools."""
    return [
        types.Tool(
            name="add",
            description="Add two numbers",
            inputSchema={
                "type": "object",
                "properties": {
                    "a": {"type": "number", "description": "number to add"}, 
                    "b": {"type": "number", "description": "number to add"}
                },
                "required": ["query"],
            },
        )
    ]
```

**TypeScript**

```typescript
server.setRequestHandler(ListToolsRequestSchema, async (request) => {
  // ត្រឡប់បញ្ជីឧបករណ៍ដែលបានចុះបញ្ជី
  return {
    tools: [{
        name="add",
        description="Add two numbers",
        inputSchema={
            "type": "object",
            "properties": {
                "a": {"type": "number", "description": "number to add"}, 
                "b": {"type": "number", "description": "number to add"}
            },
            "required": ["query"],
        }
    }]
  };
});
```

នៅទីនេះ មានមុខងារត្រឡប់បញ្ជីមុខងារ។ រាល់ធាតុនៅក្នុងបញ្ជីឧបករណ៍មានវាលដូចជា `name`, `description` និង `inputSchema` ដែលគោរពទៅតាមប្រភេទត្រឡប់។ វាអនុញ្ញាតឲ្យយើងដាក់ឧបករណ៍ និងការបកស្រាយមុខងាររបស់យើងនៅកន្លែងផ្សេងទៀត។ ឥឡូវនេះ យើងអាចបង្កើតឧបករណ៍ទាំងអស់នៅក្នុងថត tools ហើយធ្វើដូចគ្នាជាមួយមុខងារទាំងអស់ ដូច្នេះគម្រោងរបស់អ្នកអាចត្រូវបានរៀបចំដូចខាងក្រោម៖

```text
app
--| tools
----| add
----| substract
--| resources
----| products
----| schemas
--| prompts
----| product-description
```

វាល្អណាស់ សំណង់របស់យើងអាចក្លាយទៅជាគន្លងស្អាតត្រឹមត្រូវ។

អំពីការហៅឧបករណ៍វិញវាទៅដូចម្តេច? តើវាជាគំនិតដូចគ្នា ម្ចាស់ការតែមួយសម្រាប់ហៅឧបករណ៍ មិនថាអ្នកហៅឧបករណ៍ណា? មែនហើយ។ នេះជាកូដសម្រាប់វា៖

**Python**

```python
@server.call_tool()
async def handle_call_tool(
    name: str, arguments: dict[str, str] | None
) -> list[types.TextContent]:
    
    # tools គឺជាដឹកជញ្ជូនមួយដែលមានឈ្មោះឧបករណ៍ជាគន្លង។
    if name not in tools.tools:
        raise ValueError(f"Unknown tool: {name}")
    
    tool = tools.tools[name]

    result = "default"
    try:
        result = await tool["handler"](../../../../03-GettingStarted/10-advanced/arguments)
    except Exception as e:
        raise ValueError(f"Error calling tool {name}: {str(e)}")

    return [
        types.TextContent(type="text", text=str(result))
    ] 
```

**TypeScript**

```typescript
server.setRequestHandler(CallToolRequestSchema, async (request) => {
    const { params: { name } } = request;
    let tool = tools.find(t => t.name === name);
    if(!tool) {
        return {
            error: {
                code: "tool_not_found",
                message: `Tool ${name} not found.`
            }
       };
    }
    
    // អាគពី: request.params.arguments
    // TODO ហៅឧបករណ៍,

    return {
       content: [{ type: "text", text: `Tool ${name} called with arguments: ${JSON.stringify(input)}, result: ${JSON.stringify(result)}` }]
    };
});
```

ដូចដែលអ្នកឃើញពីកូដខាងលើ យើងត្រូវបំបែកឧបករណ៍ដែលត្រូវហៅ និងអាគុយម៉ង់ដោយត្រឹមត្រូវ ហើយបន្ទាប់មកត្រូវធ្វើដំណើរការហៅឧបករណ៍។

## កែលម្អវិធីសាស្រ្តជាមួយការត្រួតពិនិត្យ

រហូតមកដល់ពេលនេះ អ្នកបានឃើញពីរបៀបទាំងពីរសម្រាប់ចុះបញ្ជីឧបករណ៍ ធនធាន និងពន្លឺ អាចជំនួសបាន ។ តើតើយើងត្រូវធ្វើអ្វីបន្ថែម? យើងគួរតែបន្ថែមវិធីសាស្រ្តមួយសម្រាប់ត្រួតពិនិត្យតម្កល់ទុកថាឧបករណ៍ត្រូវបានហៅជាមួយអាគុយម៉ង់ត្រឹមត្រូវ។ រាល់ runtime មានដំណោះស្រាយផ្ទាល់ខ្លួនសម្រាប់រឿងនេះ ដូចជា Python ប្រើ Pydantic និង TypeScript ប្រើ Zod។ គំនិតគឺយើងធ្វើដូចខាងក្រោម៖

- ផ្លាស់ទីលូហ្វូសម្រាប់បង្កើតមុខងារ (ឧបករណ៍, ធនធាន ឬពន្លឺ) ទៅថតផ្ទាល់ខ្លួន។
- បន្ថែមវិធីសាស្រ្តមួយសម្រាប់ត្រួតពិនិត្យសំណើចូលមក មិនឲ្យមានករណីខុសត្រូវ ឧទាហរណ៍ហៅឧបករណ៍។

### បង្កើតមុខងារ

ដើម្បីបង្កើតមុខងារ យើងត្រូវបង្កើតឯកសារសម្រាប់មុខងារនោះ ហើយធានាថាវាមានវាលសំខាន់ដែលមុខងារនោះត្រូវការ។ វាលខុសគ្នា​បន្តិច​នៅចន្លោះឧបករណ៍ ធនធាន និងពន្លឺ។

**Python**

```python
# schema.py
from pydantic import BaseModel

class AddInputModel(BaseModel):
    a: float
    b: float

# add.py

from .schema import AddInputModel

async def add_handler(args) -> float:
    try:
        # ផ្ទៀងផ្ទាត់ការបញ្ចូលដោយប្រើម៉ូឌែល Pydantic
        input_model = AddInputModel(**args)
    except Exception as e:
        raise ValueError(f"Invalid input: {str(e)}")

    # TODO: បន្ថែម Pydantic ដើម្បីយើងអាចបង្កើត AddInputModel និងផ្ទៀងផ្ទាត់ args បាន

    """Handler function for the add tool."""
    return float(input_model.a) + float(input_model.b)

tool_add = {
    "name": "add",
    "description": "Adds two numbers",
    "input_schema": AddInputModel,
    "handler": add_handler 
}
```

នៅទីនេះ អ្នកអាចមើលឃើញយើងធ្វើដូចខាងក្រោម៖

- បង្កើតស្គីមាមួយដោយប្រើ Pydantic `AddInputModel` ដែលមានវាល `a` និង `b` នៅក្នុងឯកសារ *schema.py*។
- ព្យាយាមបំលែងសំណើចូលឲ្យយល់យ៉ាងដូចជា `AddInputModel` ប្រសិនបើparameters មិនត្រូវគ្នា វានឹងបរាជ័យ៖

   ```python
   # add.py
    try:
        # បញ្ចាក់ចូលប្រើប្រាស់ដោយប្រើម៉ូដែល Pydantic
        input_model = AddInputModel(**args)
    except Exception as e:
        raise ValueError(f"Invalid input: {str(e)}")
   ```

អ្នកអាចជ្រើសរើសត្រូវដាក់ logic ផ្ទាល់នៃការបំលែងនេះនៅក្នុងមុខងារហៅឧបករណ៍ ឬនៅក្នុងម្ចាស់ការផងដែរ។

**TypeScript**

```typescript
// server.ts
server.setRequestHandler(CallToolRequestSchema, async (request) => {
    const { params: { name } } = request;
    let tool = tools.find(t => t.name === name);
    if (!tool) {
       return {
        error: {
            code: "tool_not_found",
            message: `Tool ${name} not found.`
        }
       };
    }
    const Schema = tool.rawSchema;

    try {
       const input = Schema.parse(request.params.arguments);

       // @ts-ignore
       const result = await tool.callback(input);

       return {
          content: [{ type: "text", text: `Tool ${name} called with arguments: ${JSON.stringify(input)}, result: ${JSON.stringify(result)}` }]
      };
    } catch (error) {
       return {
          error: {
             code: "invalid_arguments",
             message: `Invalid arguments for tool ${name}: ${error instanceof Error ? error.message : String(error)}`
          }
    };
   }

});

// schema.ts
import { z } from 'zod';

export const MathInputSchema = z.object({ a: z.number(), b: z.number() });

// add.ts
import { Tool } from "./tool.js";
import { MathInputSchema } from "./schema.js";
import { zodToJsonSchema } from "zod-to-json-schema";

export default {
    name: "add",
    rawSchema: MathInputSchema,
    inputSchema: zodToJsonSchema(MathInputSchema),
    callback: async ({ a, b }) => {
        return {
            content: [{ type: "text", text: String(a + b) }]
        };
    }
} as Tool;
```

- នៅក្នុងម្ចាស់ការដែលទទួលខុសត្រូវលើការហៅឧបករណ៍ទាំងអស់ ឥឡូវនេះយើងព្យាយាមបំលែងសំណើចូលទៅក្នុងស្គីមដែលកំណត់របស់ឧបករណ៍៖

    ```typescript
    const Schema = tool.rawSchema;

    try {
       const input = Schema.parse(request.params.arguments);
    ```

ប្រសិនបើវាដំណើរការ យើងបន្តទៅកាន់ការហៅឧបករណ៍ពិតប្រាកដ៖

    ```typescript
    const result = await tool.callback(input);
    ```

ដូចដែលអ្នកឃើញ វិធីសាស្រ្តនេះបង្កើតសំណង់ល្អ ដូច្នេះគ្រប់អ្វីមានទីតាំងផ្ទាល់ខ្លួន, *server.ts* គឺជា​ឯកសារតិចបំផុតដែលភ្ជាប់ម្ចាស់ការសំណើ និងមុខងារនីមួយៗស្ថិតក្នុងថតរបស់ពួកគេ ដូចជា tools/, resources/ ឬ /prompts។

ល្អហើយ យើងនឹងព្យាយាមកសាងវាបន្តទៀត។

## ផ្សែងសមីការ៖ បង្កើតម៉ាស៊ីនបម្រើកម្រិតទាប

ក្នុង​ផ្សែងនេះ យើងនឹងធ្វើដូច្នេះ៖

1. បង្កើតម៉ាស៊ីនបម្រើកម្រិតទាប ដែលទទួលខុសត្រូវបញ្ជីឧបករណ៍ និងការហៅឧបករណ៍។
1. អនុវត្តសំណង់ដែលអ្នកអាចពង្រីកបាន។
1. បន្ថែមការត្រួតពិនិត្យ ឲ្យធានាថាការហៅឧបករណ៍ត្រូវបានត្រួតពិនិត្យយ៉ាងត្រឹមត្រូវ។

### -1- បង្កើតសំណង់

របស់ដំបូងដែលយើងត្រូវដោះស្រាយ គឺសំណង់មួយដែលជួយឲ្យយើងអាចពង្រីកបានពេលបន្ថែមមុខងារថ្មីៗ។ វាហាក់ដូចជា៖

**Python**

```text
server.py
--| tools
----| __init__.py
----| add.py
----| schema.py
client.py
```

**TypeScript**

```text
server.ts
--| tools
----| add.ts
----| schema.ts
client.ts
```

ឥឡូវយើងបានដាក់សំណង់ដែលធានាថាយើងអាចបន្ថែមឧបករណ៍ថ្មីៗនៅក្នុងថត tools បានយ៉ាងងាយស្រួល។ អ្នកអាចបន្ថែមថតរងសម្រាប់ធនធាន និងពន្លឺបានដែរ។

### -2- បង្កើតឧបករណ៍

មកមើលរបៀបបង្កើតឧបករណ៍។ ជាចម្បង វាត្រូវបានបង្កើតនៅក្នុងថតរង *tool* ដូចខាងក្រោម៖

**Python**

```python
from .schema import AddInputModel

async def add_handler(args) -> float:
    try:
        # អំណោយផលបញ្ចូលដោយប្រើម៉ូដែល Pydantic
        input_model = AddInputModel(**args)
    except Exception as e:
        raise ValueError(f"Invalid input: {str(e)}")

    # TODO: បន្ថែម Pydantic ដើម្បី​យើង​អាច​បង្កើត AddInputModel ហើយ​ផ្ទៀងផ្ទាត់ args

    """Handler function for the add tool."""
    return float(input_model.a) + float(input_model.b)

tool_add = {
    "name": "add",
    "description": "Adds two numbers",
    "input_schema": AddInputModel,
    "handler": add_handler 
}
```

អ្វីដែលយើងឃើញនៅទីនេះគឺរបៀបកំណត់ឈ្មោះ ពិពណ៌នា និងស្គីមចូលដោយប្រើ Pydantic ហើយមានម្ចាស់ការដែលនឹងត្រូវហៅនៅពេលឧបករណ៍នេះត្រូវបានហៅ។ ចុងក្រោយយើងបង្ហាញ `tool_add` ដែលជាដិចស្យូនារីមួយដែលមានគ្រប់លក្ខណៈទាំងនេះ។

មាន *schema.py* ផងដែរ ដែលប្រើសម្រាប់កំណត់ស្គីមចូលរបស់ឧបករណ៍៖

```python
from pydantic import BaseModel

class AddInputModel(BaseModel):
    a: float
    b: float
```

ចាំបាច់ត្រូវបំពេញ *__init__.py* ដើម្បីធ្វើឲ្យថត tools ត្រូវបានគេដឹងថាជាម៉ូឌុលមួយ។ បន្ថែមពីនេះ យើងត្រូវបង្ហាញម៉ូឌុលខាងក្នុងដូចខាងក្រោម៖

```python
from .add import tool_add

tools = {
  tool_add["name"] : tool_add
}
```

យើងអាចបន្ថែមទៅឯកសារនេះបន្តបន្ទាប់តាមរយៈការបន្ថែមឧបករណ៍ថ្មីៗ។

**TypeScript**

```typescript
import { Tool } from "./tool.js";
import { MathInputSchema } from "./schema.js";
import { zodToJsonSchema } from "zod-to-json-schema";

export default {
    name: "add",
    rawSchema: MathInputSchema,
    inputSchema: zodToJsonSchema(MathInputSchema),
    callback: async ({ a, b }) => {
        return {
            content: [{ type: "text", text: String(a + b) }]
        };
    }
} as Tool;
```

នៅទីនេះ យើងបង្កើតដិចស្យូនារីមួយដែលមានគុណលក្ខណៈ៖

- name, នេះជាឈ្មោះឧបករណ៍។
- rawSchema, នេះគឺជាស្គីម Zod ដែលប្រើត្រួតពិនិត្យសំណើចូលសម្រាប់ហៅឧបករណ៍នេះ។
- inputSchema, ស្គីមនេះនឹងត្រូវបានប្រើដោយម្ចាស់ការ។
- callback, ប្រើសម្រាប់ហៅឧបករណ៍។

មាន `Tool` ដែលប្រើបម្លែងដិចស្យូនារីនេះទៅជាប្រភេទដែលម្ចាស់ការម៉ាស៊ីនបម្រើ mcp អាចទទួលបាន ហើយវាមើលទៅដូចជាៈ

```typescript
import { z } from 'zod';

export interface Tool {
    name: string;
    inputSchema: any;
    rawSchema: z.ZodTypeAny;
    callback: (args: z.infer<z.ZodTypeAny>) => Promise<{ content: { type: string; text: string }[] }>;
}
```

ហើយមាន *schema.ts* ដែលយើងរក្សាទុកស្គីមចូលសម្រាប់ឧបករណ៍នីមួយៗ ដូចខាងក្រោម មានស្គីមតែមួយសម្រាប់ឥឡូវនេះ ប៉ុន្តែពេលយើងបន្ថែមឧបករណ៍ យើងអាចបន្ថែមបញ្ចូលថ្មីៗបាន៖

```typescript
import { z } from 'zod';

export const MathInputSchema = z.object({ a: z.number(), b: z.number() });
```

ល្អហើយ យើងត្រៀមខ្លួនដើម្បីដោះស្រាយបញ្ជីឧបករណ៍បន្ទាប់។

### -3- ដំណើរការបញ្ជីឧបករណ៍

បន្ទាប់មក ដើម្បីដំណើរការបញ្ជីឧបករណ៍ យើងត្រូវតាំងម្ចាស់ការសំណួរសម្រាប់វា។ នេះជាអ្វីដែលយើងត្រូវបន្ថែមទៅឯកសារម៉ាស៊ីនបម្រើ៖

**Python**

```python
# កូដបានលុបចោលសម្រាប់ភាពខ្លី
from tools import tools

@server.list_tools()
async def handle_list_tools() -> list[types.Tool]:
    tool_list = []
    print(tools)

    for tool in tools.values():
        tool_list.append(
            types.Tool(
                name=tool["name"],
                description=tool["description"],
                inputSchema=pydantic_to_json(tool["input_schema"]),
            )
        )
    return tool_list
```

នៅទីនេះ យើងបន្ថែម decorator `@server.list_tools` និងមុខងារ `handle_list_tools` ដើម្បីអនុវត្ត។ នៅក្នុងមុខងារនេះ យើងត្រូវបង្កើតបញ្ជីឧបករណ៍។ ចំណាំថា អ្វីៗគ្រប់យ៉ាងនៅក្នុងឧបករណ៍ត្រូវតែមានឈ្មោះ ពិពណ៌នា និង inputSchema។  

**TypeScript**

ដើម្បីតាំងម្ចាស់ការសំណួរដើម្បីបញ្ជីឧបករណ៍ យើងត្រូវហៅ `setRequestHandler` លើម៉ាស៊ីនបម្រើដោយផ្តល់ស្គីមសមនឹងអ្វីដែលយើងកំពុងធ្វើ នៅក្នុងករណីនេះ គឺ `ListToolsRequestSchema`។

```typescript
// index.ts
import addTool from "./add.js";
import subtractTool from "./subtract.js";
import {server} from "../server.js";
import { Tool } from "./tool.js";

export let tools: Array<Tool> = [];
tools.push(addTool);
tools.push(subtractTool);

// server.ts
// កូដត្រូវបានលុបចេញសម្រាប់ខ្លី
import { tools } from './tools/index.js';

server.setRequestHandler(ListToolsRequestSchema, async (request) => {
  // បញ្ជីឧបករណ៍ដែលបានចុះបញ្ជីត្រូវបានត្រឡប់វិញ
  return {
    tools: tools
  };
});
```

ល្អហើយ ឥឡូវយើងដោះស្រាយការបញ្ជីឧបករណ៍បានរួចហើយ បន្ទាប់មកមកមើលរបៀបហៅឧបករណ៍។

### -4- ដំណើរការហៅឧបករណ៍

ដើម្បីហៅឧបករណ៍ យើងត្រូវតាំងម្ចាស់ការសំណួរផ្សេងទៀត ដែលផ្តោតលើបញ្ជាក់មុខងារដែលត្រូវហៅ និងអាគុយម៉ង់ដែលប្រើ។

**Python**

យើងប្រើ decorator `@server.call_tool` ហើយអនុវត្តវាជាមុខងារដូចជា `handle_call_tool`។ នៅក្នុងមុខងារនេះ យើងត្រូវបំបែកឈ្មោះឧបករណ៍ អាគុយម៉ង់ និងធានាថា អាគុយម៉ង់ត្រឹមត្រូវសម្រាប់ឧបករណ៍នោះ។ អ្នកអាចផ្ទៀងផ្ទាត់អាគុយម៉ង់នៅក្នុងមុខងារនេះ ឬនៅក្នុងឧបករណ៍ផ្ទាល់។

```python
@server.call_tool()
async def handle_call_tool(
    name: str, arguments: dict[str, str] | None
) -> list[types.TextContent]:
    
    # tools គឺជាថតលេខមួយដែលមានឈ្មោះឧបករណ៍ជាគ្រាប់សោ
    if name not in tools.tools:
        raise ValueError(f"Unknown tool: {name}")
    
    tool = tools.tools[name]

    result = "default"
    try:
        # រំដោះឧបករណ៍
        result = await tool["handler"](../../../../03-GettingStarted/10-advanced/arguments)
    except Exception as e:
        raise ValueError(f"Error calling tool {name}: {str(e)}")

    return [
        types.TextContent(type="text", text=str(result))
    ] 
```

នេះគឺអ្វីដែលកើតឡើង៖

- ឈ្មោះឧបករណ៍របស់យើងមានជាមុនក្នុង parameter ចូល `name` ដែលផ្ទុក argument នៅក្នុង dictionary `arguments`។

- ឧបករណ៍ត្រូវបានហៅជាមួយ `result = await tool["handler"](../../../../03-GettingStarted/10-advanced/arguments)`។ ការត្រួតពិនិត្យ argument ស្ថិតក្នុងគុណលក្ខណៈ `handler` ដែលបង្ហាញទៅមុខងារ ហើយបើបរាជ័យវានឹងបង្ហាញករណីកំហុស។

ប៉ុន្ដេះ ឥឡូវនេះយើងមានការយល់ដឹងពេញលេញអំពីការបញ្ជី និងការហៅឧបករណ៍ដោយប្រើម៉ាស៊ីនបម្រើកម្រិតទាប។

មើល [ឧទាហរណ៍ពេញលេញ](./code/README.md) នៅទីនេះ

## កិច្ចការណ៍

ពង្រឹងកូដដែលអ្នកបានទទួលជាមួយឧបករណ៍ ធនធាន និងពន្លឺជាច្រើន ហើយពិចារណាថាអ្នកត្រូវបន្ថែមថតតែក្នុងស្ថានីយ tools ប៉ុណ្ណោះ មិនត្រូវបន្ថែមកន្លែងផ្សេងទេ។

*គ្មានដំណោះស្រាយ*

## សង្ខេប

នៅជំពូកនេះ យើងបានឃើញរបៀបប្រើម៉ាស៊ីនបម្រើកម្រិតទាប និងរបៀបដែលវាអាចជួយបង្កើតសំណង់ល្អ ដែលយើងអាចបន្ដពង្រីកបាន។ យើងក៏បានពិភាក្សាអំពីការត្រួតពិនិត្យ និងបានបង្ហាញពីរបៀបប្រើបណ្ណាល័យត្រួតពិនិត្យដូចជា Pydantic និង Zod ដើម្បីបង្កើតស្គីមសម្រាប់ការត្រួតពិនិត្យចូល។

## អ្វីជាដើមបន្ទាប់

- បន្ទាប់: [សមត្ថភាពសម្រួលត្រឹមត្រូវ](../11-simple-auth/README.md)

---

<!-- CO-OP TRANSLATOR DISCLAIMER START -->
**ការបដិយាករណ៍**:  
ឯកសារនេះត្រូវបានបកប្រែដោយប្រើសេវាបកប្រែ AI [Co-op Translator](https://github.com/Azure/co-op-translator)។ ខណៈពេលយើងខិតខំរកភាពត្រឹមត្រូវ សូមយល់ថាការបកប្រែដោយស្វ័យប្រវត្តអាចមានកំហុស ឬការខ្វះខាត។ ឯកសារដើមក្នុងភាសាដែលមានដំណើរការជារដ្ឋបាលគឺជាអំពើតុល្យភាពសំខាន់។ សម្រាប់ព័ត៌មានដែលមានសារៈសំខាន់ ខ្ញុំណែនាំឲ្យប្រើការបកប្រែដោយមនុស្សវិជ្ជាជីវៈ។ យើងមិនទទួលខុសត្រូវចំពោះការយល់ច្រឡំ ឬការបកស្រាយខុសពីការប្រើប្រាស់ការបកប្រែនេះឡើយ។
<!-- CO-OP TRANSLATOR DISCLAIMER END -->