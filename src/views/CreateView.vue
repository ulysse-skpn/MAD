<script>
    const identification = 
    {
        "lastName" : "Nom",
        "firstName" : "Prénom",
        "job" : "Fonction",
        "service" : "Service",
        "equipment" : "Type de matériel",
        "model" : "Modèle",
        "brand" : "Marque",
        "quantity" : "Quantité",
        "condition" : "Etat",
        "signature" : "Signature"
    }

    export default 
    {
        data() {
            return {
                errors:[],
                email:"",
                quantity: 1,
                form:
                {
                    lastName: "",
                    firstName: "",
                    job: "",
                    service: null,
                    equipment: null,
                    model: "",
                    brand: "",
                    serialNumber: "",
                    condition: null,
                    attachment: null,
                    signature: null
                },
                service_selected: '0',
                service_option:
                [
                    {text: "-- Choix du service --" , value: "0"},
                    {text: "Finance" , value: "finance"},
                    {text: "Informatique" , value: "informatique"},
                    {text: "Ressources Humaines" , value: "rh"},
                    {text: "Service généraux" , value: "sg"},
                ],
                equipment_selected: '0',
                equipment_option:
                [
                    {text: "-- Choix du matériel --" , value: "0"},
                    {text: "Clavier" , value: "clavier"},
                    {text: "Souris" , value: "souris"},
                    {text: "Enceintes" , value: "enceintes"},
                    {text: "Dock" , value: "dock"},
                ],
                condition_selected: '0',
                condition_option:
                [
                    {text: "-- Etat --" , value: "0"},
                    {text: "Neuf" , value: "neuf"},
                    {text: "Bon état" , value: "bon"},
                    {text: "passable" , value: "passable"}
                ]
            }
        },
        mounted() 
        {
            const canvas = document.querySelector("#signature")
            const ctx = canvas.getContext('2d');

            let isDrawing = false;
            let lastX = 0;
            let lastY = 0;

            function draw(e) 
            {
                if (!isDrawing) return;
                ctx.beginPath();
                ctx.moveTo(lastX, lastY);
                ctx.lineTo(e.offsetX, e.offsetY);
                ctx.stroke();
                [lastX, lastY] = [e.offsetX, e.offsetY];
            }

            canvas.addEventListener('mousedown', (e) => {
                isDrawing = true;
                [lastX, lastY] = [e.offsetX, e.offsetY];
            });

            canvas.addEventListener('mousemove', draw);
            canvas.addEventListener('mouseup', () => isDrawing = false);
            canvas.addEventListener('mouseout', () => isDrawing = false);


        },
        watch: {
            email(value)
            {
                this.email = value
                this.validateEmail(value);
            }
        },
        methods: 
        {
            updateTextInput(e) 
            {
                const newVal = e.target.value;
                this.form.quantity = newVal
            },
            uploadFile()
            {
                const input = document.querySelector('#attachment')
                const file = input.files[0]
                this.form.attachment = file
            },
            validateEmail(email)
            {
                if (/^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(email)) this.errorMsg = ""
                else this.errorMsg = "L'email n'est pas bien formaté"
            },
            checkForm()
            {
                this.errors = []

                const canvas = document.querySelector('#signature')
                let isCanvasBlank = this.isCanvasBlank(canvas)

                if( this.form.lastName !== "" 
                    && this.form.firstName !== "" 
                    && this.email !== "" 
                    && this.form.job !== "" 
                    && this.service_selected !== "0" 
                    && this.equipment_service_selected !== "0"
                    && this.form.model !== ""
                    && this.form.brand !== ""
                    && this.condition_service_selected !== "0"
                    && isCanvasBlank === false 
                ) return true

                for (const key in this.form ) 
                {
                    if (Object.hasOwnProperty.call(this.form, key)) 
                    {

                        const element = this.form[key];

                        if( key === "service" && this.service_selected === "0" 
                            || key === "equipment" && this.equipment_selected === "0"
                            || key === "condition" && this.condition_selected === "0"  
                        )
                        {
                            this.errors.push( identification[key] + " : Selectionnez un des choix disponibles")
                        }
                        else if( element === "" && Object.keys(identification).includes(key) ) this.errors.push( identification[key] + " : Ce champ est requis")
                        else if( key === "signature" && isCanvasBlank ) this.errors.push( identification[key] + " : Une signature est nécessaire")
                    }
                }

                return false
            },
            isCanvasBlank(canvas) 
            {
                return !canvas.getContext('2d')
                    .getImageData(0, 0, canvas.width, canvas.height).data
                    .some(channel => channel !== 0);
            },
            testForm(e)
            {
                e.preventDefault()
                this.form.lastName = "François"
                this.form.firstName = "Jean"
                this.email = "jean@francois.fr"
                this.form.job = "Développeur"
                this.service_selected = "rh"
                this.equipment_selected = "clavier"
                this.form.model = "Modèle A"
                this.form.brand = "Epson"
                this.condition_selected = "neuf"
            },
            submitForm()
            {
                if( !this.checkForm() ) return

                this.form.service = this.service_selected
                this.form.equipment = this.equipment_selected
                this.form.condition = this.condition_selected
                this.form.quantity = this.quantity

                const form = Object.fromEntries(
                    Object.entries(this.form).map(([key, value]) => {
                        return [key, typeof value === 'string' ? value.toLowerCase() : value];
                    })
                );

                console.log( form )
            }
        }
    }
</script>

<template>
    
    <div class="wrapper">
        <div class="content bg-light">
            <div class="container">
                <div class="row">
                    <div class="form-wrapper">
                        <form id="formulaire">
    
                            <div class="row input-wrapper">
                                
                                <!-- NOM DEMANDEUR -->
                                <div class="col-6">
                                    <label for="borrower-lastName" class="form-label">Nom <span class="text-danger">*</span></label>
                                    <input v-model="form.lastName" type="text" class="form-control" id="borrower-lastName" name="lastName" placeholder="François" required>
                                </div>
    
                                <!-- PRENOM DEMANDEUR -->
                                <div class="col-6">
                                    <label for="borrower-firstName" class="form-label">Prénom <span class="text-danger">*</span></label>
                                    <input v-model="form.firstName" type="text" class="form-control" id="borrower-firstName" name="firstName" placeholder="Jean" required>
                                </div>
    
                                <!-- MAIL -->
                                <div class="col-6">
                                    <label for="email" class="form-label">Email <span class="text-danger">*</span></label>
                                    <input v-model="email" type="email" class="form-control" id="email" name="email" placeholder="nom@prenom.fr" required>
                                </div>
    
                                <!-- FONCTION DEMANDEUR -->
                                <div class="col-6">
                                    <label for="borrower-job" class="form-label">Fonction <span class="text-danger">*</span></label>
                                    <input v-model="form.job" type="text" class="form-control" id="borrower-job" name="job" placeholder="Développeur" required>
                                </div>
    
                                <!-- SERVICE DEMANDEUR -->
                                <div class="col-6">
                                    <label for="borrower-department" class="form-label">Service <span class="text-danger">*</span></label>
                                    <!-- <select v-model="form.service" class="form-control" name="department" id="borrower-department" required> -->
                                    <select v-model="service_selected" class="form-control" name="department" id="borrower-department" required>
                                        <option v-for="option in service_option" :value="option.value" :key="option">{{ option.text }}</option>
                                    </select>
                                </div>
    
                                <!-- TYPE DE MATERIEL -->
                                <div class="col-6">
                                    <label for="equipment" class="form-label">Type de matériel <span class="text-danger">*</span></label>
                                    <!-- <select v-model="form.equipment" class="form-control" name="equipment" id="equipment" required> -->
                                    <select v-model="equipment_selected" class="form-control" name="equipment" id="equipment" required>
                                        <option v-for="option in equipment_option" :value="option.value" :key="option">{{ option.text }}</option>
                                    </select>
                                </div>
    
                                <!-- MODELE -->
                                <div class="col-6">
                                    <label for="model" class="form-label">Modèle <span class="text-danger">*</span></label>
                                    <input v-model="form.model" type="text" class="form-control" id="model" name="model" placeholder="Modèle R-..." required>
                                </div>
    
                                <!-- MARQUE -->
                                <div class="col-6">
                                    <label for="brand" class="form-label">Marque <span class="text-danger">*</span></label>
                                    <input v-model="form.brand" type="text" class="form-control" id="brand" name="brand"  required>
                                </div>
    
                                <!-- QUANTITE -->
                                <div class="col-6">
                                    <label for="quantity" class="form-label">Quantity </label>
                                    <input type="range" v-model.number="quantity" class="form-range" id="quantity" name="quantity" min="1" max="15" step="1" v-on:change="updateTextInput"> <span id="rangeValue">{{quantity}}</span>
                                </div>
    
                                <!-- NUMERO DE SERIE -->
                                <div class="col-6">
                                    <label for="serialNumber" class="form-label">Numéro de série</label>
                                    <input v-model="form.serialNumber" type="text" class="form-control" id="serialNumber" name="serial-number">
                                </div>
    
                                <!-- ETAT -->
                                <div class="col-6">
                                    <label for="condition" class="form-label">Etat <span class="text-danger">*</span></label>
                                    <!-- <select v-model="form.condition" class="form-control" name="condition" id="condition" required> -->
                                    <select v-model="condition_selected" class="form-control" name="condition" id="condition" required>
                                        <option v-for="option in condition_option" :value="option.value" :key="option">{{ option.text }}</option>
                                    </select>
                                </div>
    
                                <!-- PIECE JOINTE -->
                                <div class="col-6">
                                    <label for="attachment" class="form-label">Pièce jointe</label>
                                    <input v-on:change="uploadFile" type="file" class="form-control" id="attachment" name="attachment">
                                </div>
    
                                <!-- SIGNATURE -->
                                <div class="col-4 mb-0">
                                    <label for="signature" class="form-label">Signature <span class="text-danger">*</span></label>
                                    <br>
                                    <canvas class="canvas" id="signature">
    
                                    </canvas>
                                </div>
    
                                <div class="confirm-buttons mb-0 col-8">
                                    <div class="d-flex justify-content-center">
                                        <button type="button" class="btn btn-danger buttons px-3 py-2" @click="submitForm">Enregistrer</button>
                                        <button class="btn btn-warning text-white buttons px-3 py-2" >Brouillon</button>
                                        
                                        <!-- Bouton de test servant à préremplir le formulaire -->
                                        <button class="btn btn-info text-white buttons" @click="testForm">Préremplir</button>
                                    </div>
    
                                    <ul class="errors">
                                        <li class="text-danger" v-for="error in errors" :key=error>{{ error }}</li>
                                    </ul>
                                </div>
                            </div>
    
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>   

<style scoped>
    .wrapper
    {
        background: no-repeat url("./../assets/background/form-bg.webp");
        height: 100vh;
    }
    .content
    {
        position: fixed;
        top: 0;
        right: 0;
        left: 25rem;
        font-size: 0.7rem;
        height: 100%;
        background-color: transparent;
    }
    .form-wrapper
    {
        padding: 2.5rem 5rem 0rem 10rem;
        height: 100vh;
    }

    .input-wrapper div
    {
        font-weight: bold;
        padding: 0 1.5rem;
        margin: 0.5rem 0;
    }

    .canvas
    {
        border: 2px grey solid;
    }

    .buttons
    {
        margin: 0 0.5rem;
    }

    .errors
    {
        height: 100px;
        display: flex;
        flex-direction: column;
        flex-wrap: wrap;
    }
</style>