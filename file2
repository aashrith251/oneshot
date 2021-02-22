import React, { Component } from 'react'
import {Button, Row, Col} from 'react-bootstrap'
import {Pie, Bar} from 'react-chartjs-2'
import ReactSelect from './ReactSelect'
import TableHeader from './TableHeader'

//AXIOS
import instance from '../axios'


export default class Charts extends Component {
    constructor(){
        super();

        this.handleChartGenerator = this.handleChartGenerator.bind(this);
        this.handleCourseChange = this.handleCourseChange.bind(this);
        this.handleChartCategoryChange = this.handleChartCategoryChange.bind(this);
        this.handleStateChange = this.handleStateChange.bind(this);

        this.state = {
            chartcategory: [{value: "courses", label:"Courses"}, {value: "states", label:"States"}],
            selectedchartcategory: '',
            collegecoursesdata: [],
            courseslabels: [],
            selectedcourse: '',
            showchart: Boolean(false),
            showcoursechart: 0,
            showstatechart: 0,
            specificcoursecolleges: [],
            specificcoursecollegesheader: ['Name', 'No. of Students', 'Location', 'Courses Offered','Year Founded'],
            showspecificcoursetable: 0,
            stateoptions: [],
            showspecificstatetable: 0,
            specificstatecolleges: [],
            courseschartdata: {
                labels: [],
                datasets: [
                    {
                        label: 'Courses',
                        backgroundColor: [
                            '#B21F00',
                            '#C9DE00',
                            '#2FDE00',
                            '#00A6B4',
                            '#6800B4',
                            '#6855B4'
                        ],
                        hoverBackgroundColor: [
                            '#501800',
                            '#4B5000',
                            '#175000',
                            '#003350',
                            '#35014F',
                            '#35934F'
                        ],
                        data: []
                    }
                ]
            },
            statechartdata: {
                labels: [],
                datasets: [
                    {
                        label: 'States',
                        fill: false,
                        lineTension: 0.5,
                        backgroundColor: 'rgba(75,192,192,1)',
                        borderColor: 'rgba(0,0,0,1)',
                        borderWidth: 2,
                        data: []
                    }
                ]
            }
        }
    }

    handleChartCategoryChange = selectedchartcategory => {
        this.setState({selectedchartcategory});
    }

    handleChartGenerator() {

        if(this.state.selectedchartcategory.value==='courses') {
            instance.get('/college/chart/courses')
                .then((res)=>{
                    
                    const data = {...this.state.courseschartdata.datasets[0], data: res.data.percentage};
                    // const label = {...this.state.courseschartdata, labels: res.data.label};
                    const newchartdata = {...this.state.courseschartdata, labels: res.data.label, datasets: [data]};
                    this.setState({
                        courseschartdata: newchartdata,
                        showchart: Boolean(true),
                        showcoursechart: 1,
                        showstatechart:0,
                        courseslabels: res.data.label.map((labels)=>{
                            return({
                                value: labels,
                                label: labels
                            })
                        })
                    })
                    console.log(this.state.courseslabels);
                })
        }

        if(this.state.selectedchartcategory.value==='states') {
            instance.get('/college/collegedata/chart')
            .then((res)=>{
                const data = {...this.state.statechartdata.datasets[0], data: res.data.percentage};
                    // const label = {...this.state.courseschartdata, labels: res.data.label};
                const newchartdata = {...this.state.statechartdata, labels: res.data.label, datasets: [data]};
                this.setState({
                    statechartdata: newchartdata,
                    showstatechart: 1,
                    showcoursechart: 0,
                    stateoptions: res.data.label.map((labels)=>{
                        return({
                            value: labels,
                            label: labels
                        })
                    })
                })
            })
        }

    }

    handleCourseChange = selectedcourse => {
        this.setState({selectedcourse});
        let selectcourseurl = '/college/courses/'+selectedcourse.value;
        instance.get(selectcourseurl)
            .then((res)=>{
                this.setState({
                    specificcoursecolleges: res.data,
                    showspecificcoursetable: 1
                })
            })
    }

    handleStateChange = selectedstate => {
        this.setState({selectedstate});
        let stateurl  = '/college/collegedata/chart/'+selectedstate.value;
        instance.get(stateurl)
            .then((res)=>{
                this.setState({
                    specificstatecolleges: res.data,
                    showspecificstatetable: 1
                })
            })
    }
    
    render() {
        return (
            <div className="mt-5 mb-5">
                <div>
                    <Row className="mb-3">
                        <Col md="4">
                            <p>Select Category:</p>
                            <ReactSelect 
                                selectedOption = {this.state.selectedchartcategory}
                                handleChange={this.handleChartCategoryChange}
                                options = {this.state.chartcategory}
                            />
                        </Col>
                    </Row>
                </div>
                <Button variant="dark" onClick={this.handleChartGenerator}>Generate Chart</Button>{' '}
                <div className="mt-3" style={{ display: this.state.showstatechart ? "block" : "none" }}>
                    <Bar
                        data={this.state.statechartdata}
                        options={{
                            title:{
                            display:true,
                            text:'State Data',
                            fontSize:20
                            },
                            legend:{
                            display:true,
                            position:'bottom'
                            }
                        }}
                    />
                    <div className="mt-4" style={{ display: this.state.showstatechart ? "block" : "none" }}>
                        <Row>
                            <Col md="4">
                                <p>Select State:</p>
                                <ReactSelect 
                                    selectedOption = {this.state.selectedstate}
                                    handleChange={this.handleStateChange}
                                    options = {this.state.stateoptions}
                                />
                            </Col>
                        </Row>
                    </div>

                    <div className="mt-4" style={{ display: this.state.showspecificstatetable ? "block" : "none" }}>
                        <TableHeader
                            header={this.state.specificcoursecollegesheader} 
                            data = {this.state.specificstatecolleges}
                            dataname = "College"
                        />  
                    </div>
                </div>

                <div className="mt-3" style={{ display: this.state.showcoursechart ? "block" : "none" }}>
                    <Pie 
                        data={this.state.courseschartdata}
                        options={{
                            title:{
                            display:this.state.showchart,
                            text:'Courses data',
                            fontSize:20
                            },
                            legend:{
                            display: true,
                            position:'right'
                            }
                        }}
                    />
                    <div className="mt-4" style={{ display: this.state.showchart ? "block" : "none" }}>
                        <Row>
                            <Col md="4">
                                <p>Select Course:</p>
                                <ReactSelect 
                                    selectedOption = {this.state.selectedcourse}
                                    handleChange={this.handleCourseChange}
                                    options = {this.state.courseslabels}
                                />
                            </Col>
                        </Row>
                    </div>

                    <div className="mt-4" style={{ display: this.state.showspecificcoursetable ? "block" : "none" }}>
                        <TableHeader
                            header={this.state.specificcoursecollegesheader} 
                            data = {this.state.specificcoursecolleges}
                            dataname = "College"
                        />  
                    </div>
                </div>
            </div>
        )
    }
}
