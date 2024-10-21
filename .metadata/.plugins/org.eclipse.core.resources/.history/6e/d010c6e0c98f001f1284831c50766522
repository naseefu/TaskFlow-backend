package com.task.entity;

import java.time.Duration;
import java.time.LocalDateTime;

import jakarta.persistence.Entity;
import jakarta.persistence.FetchType;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.JoinColumn;
import jakarta.persistence.ManyToOne;
import jakarta.persistence.PrePersist;
import jakarta.persistence.Table;

@Entity
@Table(name = "recruitingrequest")
public class RecruitingRequest {

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;

	@ManyToOne(fetch = FetchType.EAGER)
	@JoinColumn(name = "user_id")
	private User user;

	@ManyToOne(fetch = FetchType.EAGER)
	@JoinColumn(name = "team_id")
	private Team team;

	private String teamcode;

	private Long sendby;

	private LocalDateTime sendAt;

	
	private String agoTime;

	public LocalDateTime getSendAt() {
		return sendAt;
	}

    public void setSendAt(LocalDateTime sendAt) {
        this.sendAt = sendAt;
        this.agoTime = calculateAgoTime(sendAt);
    }

    public String getAgoTime() {
        return calculateAgoTime(this.sendAt);
    }

    private String calculateAgoTime(LocalDateTime sendAt) {
        LocalDateTime now = LocalDateTime.now();
        Duration duration = Duration.between(sendAt, now);

        long seconds = duration.getSeconds();
        if (seconds < 60) {
            return seconds + " s";
        } else if (seconds < 3600) {
            return (seconds / 60) + " min";
        } else if (seconds < 86400) {
            return (seconds / 3600) + " h";
        } else {
            return (seconds / 86400) + " day";
        }
    }

	@PrePersist
	public void prePersist() {
		this.sendAt = LocalDateTime.now();
	}

	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public User getUser() {
		return user;
	}

	public void setUser(User user) {
		this.user = user;
	}

	public Team getTeam() {
		return team;
	}

	public void setTeam(Team team) {
		this.team = team;
	}

	public String getTeamcode() {
		return teamcode;
	}

	public void setTeamcode(String teamcode) {
		this.teamcode = teamcode;
	}

	public Long getSendby() {
		return sendby;
	}

	public void setSendby(Long sendby) {
		this.sendby = sendby;
	}

}
